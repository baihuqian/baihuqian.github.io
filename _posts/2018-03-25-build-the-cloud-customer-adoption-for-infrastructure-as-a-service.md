---
layout: "post"
title: "Build the Cloud: Customer Adoption for Infrastructure-as-a-Service"
date: "2018-04-26 21:06"
tags: AWS
---
Since 2006, Amazon's cloud business, Amazon Web Services, has expanded from launching some virtual machines and offering static file storage to everything. However, the growth of customer base and revenue is still impressive. Revenue growth is still over 40% YoY for a 17 billion dollar business (12 months ending December 31, 2017). In this series of posts, I'm going to talk about my understanding on how Amazon has become the dominant player in the cloud business, and specifically, how it expanded its customer base in its Infrastructure-as-a-Service or IaaS offering.

# Why Amazon?
Amazon was not a company known for its ground-breaking, "sexy" technologies. Amazon's initial cloud offering, Elastic Compute Cloud, or EC2, use the standard virtualization technology with the open-source Xen hypervisor. It was nothing new but a fusion of existing, sophisticated solutions. But why does it dominates the cloud world later on?

The keyword is "similarity". What the customers pay for is what they already know very well: fully functional machines in a virtualized environment. There is no restriction on what the customers can do with them: they can be used for web servers, databases, batch processing, or anything. The difference between a cloud-based "instance" and a traditional machine in one's office or data center is not much, but similarity dominates. Customers could spin up some instances and immediately deploy their software to them. The adoption is easy, as long as you're OK to run your software on someone else's machine.

At that time, there was another cloud offering: Google App Engine. It was a pioneer product: you can run your code without getting a host or configuring the environment. However, the adoption was not easy. First, only a few languages were supported. If my memory served me, only Java and Python were supported when it launched in 2008. Second, there were a lot of restrictions on how the applications should be written. It did not support SQL database, nor did it support all the features from popular Java/Python frameworks such as Spring and Django. Thus, to host apps on App Engine, customers had to modify or even re-write their applications to conform to Google's design choice. Such adoption blocker dwarfs the technological advantage of the App Engine.

Amazon's cloud offering may not be the most innovative one, but it offers very low learning curve and minimum restrictions on what customers can do.

# Cloud-Native Ecosystem
In its early days, AWS was used mainly by Silicon Valley startups. These companies are so-called "cloud-native". That is, their entire production IT infrastructure is in the cloud, and they are more familiar with the cloud than traditional IT. They become more and more used to the magic cloud can bring after clicking some buttons, and they appreciate the straightforwardness and simplicity of the cloud offering than fully-featured yet expensive traditional IT equipment.

Amazon built a cloud-native ecosystem very quickly in its early years. It offers virtual machines, load balancers, databases, virtualized network isolations, monitoring and alarming, and others. Cloud was simple yet powerful. Anyone can build its business in the cloud without much effort. The cloud computing business took off quickly as we all know later on.

# Bring the Elephant to the Cloud: Where Cloud-Native Falls Short
Cloud-native is good for startups that are agile and nimble. However, their market share is small in the IT industry. The next customer group should be the big companies with large IT footprint. Their company size and demand for IT will generate large and sustainable cash flows if they move to the cloud. However, they have completely different requirements for their IT. To name a few:
1. They have complicated security and compliance requirements;
2. Their organizational structures are not so agile and nimble;
3. They have dedicated IT department with mature operational requirements, and the IT department will operate the cloud;
4. They have many mission-critical legacy systems that are hard to move;
5. They have their software license and agreement with other vendors;
1. They want to move their IT architecture to the cloud as-is, without making any changes specifically for the cloud.

Because traditional IT environment is built with fully-featured, expensive equipment, moving the architecture to the cloud requires cloud to provide feature-rich infrastructure offering as well. Cloud-native is not enough. More fully-featured products are required.

# VMWare in AWS: Why it is a Big Deal
VMWare is the leading virtualization provider to traditional IT environment. Companies that use VMWare's offerings cannot easily move to AWS because of the major differences between AWS and VMWare products. The announcement of VMWare in AWS was the critical juncture that AWS is now going after the big companies. By hosting VMWare-powered environment in the cloud, VMWare's customers can become AWS customers. Customers can use other AWS services directly with VMWare-managed instances, eliminating the need for migration.

# Challenges
Adopting big companies as customers is challenging. First, migrating customers' IT infrastructure to the cloud without downtime is like changing airplane's engine at 30000 feet. AWS's VMImport and Database Migration Service addresses the difficulty of the migration, and the adoption rate of Database Migration Service is a critical metric on the speed of customers bringing their traditional IT to the cloud. It is why you can find this metric in Amazon.Com's quarterly financial results.

Big customers have additional requirements of their IT environment. For example, certain countries require data to be stored within the border, and AWS is expanding to new geolocations at an unprecedented speed. They have security and compliance requirements, which results in the release of many advanced features. The size and the growth of cloud footprint of big companies create other challenges, such as resource management at scale, account and billing consolidations, resource sharing at scale, security and access control at scale, etc. We will continue to see AWS releases numerous features that enable big companies to adopt the cloud.
