---
layout: "post"
title: "Leetcode 355: Design Twitter"
date: "2018-08-12 16:18"
tags:
  - Leetcode
---

# Question
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

1. postTweet(userId, tweetId): Compose a new tweet.
1. getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
1. follow(followerId, followeeId): Follower follows a followee.
1. unfollow(followerId, followeeId): Follower unfollows a followee.

Example:

```
Twitter twitter = new Twitter();

// User 1 posts a new tweet (id = 5).
twitter.postTweet(1, 5);

// User 1's news feed should return a list with 1 tweet id -> [5].
twitter.getNewsFeed(1);

// User 1 follows user 2.
twitter.follow(1, 2);

// User 2 posts a new tweet (id = 6).
twitter.postTweet(2, 6);

// User 1's news feed should return a list with 2 tweet ids -> [6, 5].
// Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.getNewsFeed(1);

// User 1 unfollows user 2.
twitter.unfollow(1, 2);

// User 1's news feed should return a list with 1 tweet id -> [5],
// since user 1 is no longer following user 2.
twitter.getNewsFeed(1);
```

# Solution
```java
class Twitter {

    class UserInfo {
        int userId;
        Set<Integer> followee;
        LinkedList<TweetInfo> tweets;

        public UserInfo(int userId) {
            this.userId = userId;
            followee = new HashSet<>();
            tweets = new LinkedList<>();
        }

        public void follow(int followId) {
            if(followId != this.userId)
                followee.add(followId);
        }

        public void unfollow(int followId) {
            followee.remove(followId);
        }

        public void addPost(int tweetId, int postTime) {
            if(tweets.size() == 10)
                tweets.removeLast();
            tweets.addFirst(new TweetInfo(userId, tweetId, postTime));
        }
    }

    class TweetInfo {
        int userId;
        int tweetId;
        int postTime;

        public TweetInfo(int userId, int tweetId, int postTime) {
            this.userId = userId;
            this.tweetId = tweetId;
            this.postTime = postTime;
        }
    }

    Map<Integer, UserInfo> users;
    int time;

    /** Initialize your data structure here. */
    public Twitter() {
        users = new HashMap<>();
        time = 0;
    }

    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        users.computeIfAbsent(userId, k -> new UserInfo(userId)).addPost(tweetId, ++time);
    }

    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    public List<Integer> getNewsFeed(int userId) {
        UserInfo user = users.computeIfAbsent(userId, k -> new UserInfo(userId));
        PriorityQueue<TweetInfo> newsFeed = Twitter.newsFeedFactory();
        for(TweetInfo p: user.tweets) {
            newsFeed.offer(p);
        }
        for(int f: user.followee) {
            user = users.get(f);
            for(TweetInfo p: user.tweets) {
                newsFeed.offer(p);
            }
        }

        List<Integer> res = new ArrayList<>(10);
        for(int i = 0; i < 10; i++) {
            if(!newsFeed.isEmpty()) {
                res.add(newsFeed.poll().tweetId);
            }
            else break;
        }
        return res;
    }

    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        users.computeIfAbsent(followerId, k -> new UserInfo(followerId)).follow(followeeId);
        if(!users.containsKey(followeeId))
            users.put(followeeId, new UserInfo(followeeId));
    }

    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        users.computeIfAbsent(followerId, k -> new UserInfo(followerId)).unfollow(followeeId);
    }

    private static PriorityQueue<TweetInfo> newsFeedFactory() {
        return new PriorityQueue<>(new Comparator<TweetInfo>() {
            public int compare(TweetInfo t1, TweetInfo t2) {
                return Integer.compare(t2.postTime, t1.postTime); // reverse order
            }
        });
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```
