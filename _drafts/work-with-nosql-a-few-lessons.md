---
layout: post
title: Work with NoSQL - a Few Lessons
date: '2018-12-23 23:10'
published: true
---
Traditional relational databases are not suitable for cloud services anymore. Its vertical scaling model imposes a low scaling limit to your services, and demands complicated sharding and replication schemes to go behind the capacity of the biggest database machine. This adds complexity to your system, reduces engineering velocity, and risks your service to human-induced failure. In addition, database management for relational databases remains a complex process, even with managed database offering from cloud providers. If in-house databases are maintained, expensive DBAs must be hired and trained, and the velocity is only as fast as they can move. Even with managed database services, major version upgrades still requires manual intervention, and minor updates, though automated, still results in minor downtime and disruption to services.

Thus, more and more services are moving towards NoSQL, and even so-called "NewSQL" databases. NoSQL databases are designed for the purpose of large-scale database clustering in cloud and web applications. In these applications, requirements for performance and scalability outweighed the need for the immediate, rigid data consistency that the relational databases provide to transactional enterprise applications. Essentially, NoSQL trades the structural data and transactional operation support for much larger scale. Such shift creates challenges in design and implementations of business logic.

In recent years, so-called "NewSQL" databases start to emerge, such as Amazon Web Services' Aurora and Google Cloud's Spanner. These databases are designed to bring NoSQL's scalability and SQL's transactional support together, providing both scalability and ease of use. However, they are relatively new, less stable and mature compared to SQL and NoSQL databases. As a managed offering, they may not be availble in all geolocations, have all the features, or other constraints that hinders adoption and engineering velocity.

When I architected the data store layer for AWS Transit Gateway, contraints on NewSQL offering such as Aurora, especially regional availability, were adoption blockers. We chose Amazon DynamoDB as the data store engine as it has been in existence for close to 10 years, and its performance, availability, and scalability are exceptional. However, changing team's mindset from many years of SQL experience to key-value store was not an easy task. We have learnt a few lessons, that I humbly summarize in this post. Note that the design was done before re:Invent 2017, so features released later, e.g. DynamoDB transactions, was not incorporated.

# Organize Data by Correlation, not by Relation
In SQL world, there are many books that teach you how to break your business logic into small segments of data and relathionships among them, and translate them into tables and mapping tables. Then you add indexes to data columns to support all kinds of joins. You are not afraid of creating many tables, as long as you can efficiently join across them to produce meaningful data.

However, in NoSQL world, it becomes tricky to work with many tables at the same time. Without transactions, atomic updates across multiple items/rows in multiple tables becomes a nightmare. There are libraries out there, such as [Amazon DynamoDB Transactions](https://github.com/awslabs/dynamodb-transactions), allowing you to do that, but it is complicated and inefficinet. You will be essentially managing commit, rollback, and appliers yourself that are taken care of by the RDBMS engine, and paying overheads in operations. For example, the DynamoDb Transactions perform $$7N+4$$ operations per $$N$$ writes without contending.

Thus, key-value NoSQL can only guarantee atomic writes on a per-item level. By aggregating all closely-correlated data into the same item, transactional operations across tables and items can be eliminated. Instead of organizing the data by their intrinsic relations, you should do so by identifying clusters of highly correlated data that are loosely correlated with each other, and aggregate each cluster into one table.

One classic example of having multiple tables in the relational world is to reprensent one-to-one, one-to-many, and many-to-many relationships. Three tables are created for A, B, and the mapping between A and B. But what to do in NoSQL world?

# Composite Data or Separate Tables
There are two ways: composition or separation.

In a one-to-one relationship between $$A$$ and $$B$$, you can combine them into a single item identified with a single key. For example, let $$A$$ be a resource that your service provides to your customers, and $$B$$ be the metadata that comprises of resource $$A$$. You can simply collapse $$B$$ into $$A$$.

One-to-many relationship is a bit more complicated. NoSQL database may impose a size limit on each item. For example, DynamoDB imposes a maximum item size of 400 KB. The existence of such limit requires a well-defined upper bound of number of items collapsed into the main item. For example, if there can be no more than $$x$$ $$B$$s for each $$A$$, and the size of $$A+xB$$ is guaranteed to be within the size limit of each item, then it is recommended to collapse a list of $$B$$ into $$A$$. However, if such upper bound cannot be established, then they must be in separate tables.

One special type of tables in one-to-many relationship is the common utility table for multiple other tables. Each row in such utility table comprises a primary key of table name and identifier in each table, along with common data fields. In RDBMS a simple join can relate the data in the utility table to the main table, and the existence of such utility table helps with querying common data across many tables. However, each row only maps to a single row in another table. Therefore, it can be duplicated into each data table and eliminate the need for joins.

Many-to-many relationships are more complicated. Depends on your use case, you may want to store them in different types of NoSQL database engines, such as graph database, if the complex relationships between items are useful. Another way is to reduce them into one-to-many relationship, if one party can be grouped into static groups. Another way is to store the relationship in one side. For example, for a many-to-many relationship between $$A$$ and $$B$$, the list of $$B$$ can be stored in each $$A$$, given that there is an upper bound for number of $$B$$ mapped to each $$A$$, and queriability of list of $$A$$ mapped to each $$B$$ can be forgone.

# Think about Access Pattern
NoSQL limits the flexibility of query to a limited set of operations, and unindexed query ("scan") is far more expensive than unindexed query in RDBMS. For example, DynamoDB only supports query by Hash Key, Hash Key and Range Key (if Range Key presents), and range query by Hash Key and Range Key. These operations can be performed on the primary key and secondary indexes, but there is limit on number of secondary indexes for each table. In RDBMS, rows can be indexed by single columns or multiple columns, and indexes are free and abundant.

To effectively design your tables, especially on keys and indexes, you should think hard on the access pattern:
* What are the criteria used to locate items in a table?
* For range queries, how many distinct values are there in the range? Is it possible to convert it into several disjoint queries of same type?
* For each criteria and range query, are there any supplemental criteria that can be obtained without much effort?
* Do we care about ordering in query response?

Based on the answers, design the primary key and secondary indexes in a way that supports most of the queries by having some of them use supplemental data. For example, one query is to find the item by its resource ID. However, this query is used in an API that also provides the owner ID for this item. Then you could design a Hash Key of owner ID and Range Key of resource ID, so that it supports three use cases:
1. query by owner ID;
2. query by resource ID and owner ID (which always presents);
3. query by owner ID and a range on resource ID, such as paginated queries.

In addition, think about the consistency requirement for your access pattern.
* What kind of consistency requirement each access pattern require? If eventual consistency is enough, what is the maximum tolerance for outdated data?
* Can write operations can be broken into multiple steps such that failure occurred in any of them requires no rollback? One example is to
1. increment a counter to obtain an unused ID,
2. create a mapping for an ID, 
3. save the acutal item for the given ID. 

1 and 2 don't require rollback if 2 or 3 fails, and thus they can be broken down into separate tables/items, giving more flexibility in table designs.

# Use Primary Key for Uniqueness Guarantee
Unlike SQL database where a `UNIQUE KEY` clause provides uniqueness guarantees on arbitrary columns with no cost, uniqueness guarantee is difficult to achieve in NoSQL database, largely due to its design. NoSQL databases use distributed storage engines that are often sharded across many machines using a simple sharding scheme, and compute engines are remote to storage engines. Therefore, they don't provide uniqueness guarantees other than the primary key. In DynamoDB, only the primary key guarantees uniqueness.

If you need strong uniqueness guarantee across a column or a combination of two columns, you have no choice but make them as the primary key. You may find it awkward at the beginning since it places additional restrictions on access patterns, but you will find it is easier to deal with queries than duplicates due to race conditions or consistency issues.

# Dealing with Read Consistency
Many operations are not strong consistent in NoSQL, due to its replication and sharding scheme. For example, DynamoDB shards data by Hash Key and replicate three times for each item. The writes succeed if two of the three copies are updated. Consistent reads access at least two replicas, while inconsistent reads access only one replica.

You may choose to use consistent reads across the board. But it effectively reduces the throughput of your data store by half, or increases the cost by 100%. In many cases, you can live by with slightly outdated data by using inconsistent reads.

In addition, updates to secondary indexes may not be strongly consistent anyway. You will find read-after-write operations using secondary indexes return slightly outdated data all the time. Thus, it is important to embrace eventual consistency when using NoSQL database as you will find it everywhere. It is a losing battle trying to enforce strong consistency.

There are many ways of getting strong consistency. For example, you can define a few access patterns using consistent reads from the primary key if they **always** require strong consistency. Or you can load the items again using its primary key after reading it from secondary indexes using inconsistent reads, as primary key is immutable.

# Write Consistency
Unlike reads, write operations must be consistent, or you are at risk of overriding existing data or newer updates. One common way of enforcing write consistency is to use conditional updates.

By defining a column, `version` of numeric value, and two write operations:
* `saveNew` that sets `version = 0` and expects either the item with the given primary key or the column `version` does not exist upon write. It prevents overriding previouly-created values.
* `saveUpdate` that expects `version` to be the same as returned by the read operation, and increments the value of `version`.

A sample implementation using DynamoDB's document library is as follows:
```java
void putItemNew(String tableName, PutItemSpec spec) {
  try {
    putItemImpl(tableName, spec
                .withExpected(new Expected(VERSION).notExist())
                .withReturnValues(ReturnValue.NONE));
  } catch (ConditionalCheckFailedException e) {
    throw new ItemVersioningException("Request=" + spec.getRequest().toString(), e);
  }
}

void putItemUpdate(final String tableName, final PutItemSpec spec) {
  long curVersion = getVersion();
  setVersion(curVersion+1);
  try {
    putItemImpl(tableName, spec
                .withExpected(new Expected(VERSION).eq(curVersion))
                .withReturnValues(ReturnValue.NONE));
  } catch (ConditionalCheckFailedException e) {
    throw new ItemVersioningException("Request=" + spec.getRequest().toString(), e);
  }
}

private void putItemImpl(final String tableName, final PutItemSpec spec) {
  getTable(tableName).putItem(spec.withItem(item_));
}
```
Out-of-order writes will fail and return an `ItemVersioningException`.