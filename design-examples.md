## Design Examples

##### Twitter

Chapter 1 - two different ways of structuring user's home timeline

1. "Writes are more important than reads"

First implementation Twitter came up with

- New tweet from User A, happens gets stored in global "Tweets" table
- When User B (who follows User A) requested timeline
   - look up all tweets from "Tweets" table filtered on who User B follows
   - serve that up

Write once, read post every time from the table when timeline is requested

1. "Reads occur way more frequently than writes"

- New tweet from User A happens
- FAN-OUT FUNCTION!!
   - for each of User A's followers
      - add the new Tweet to their timelines
- When User B (who follows User A) requested timeline
   - serve up pre-calculated timeline that includes User A's new tweet

Writes are more expensive! But reads are much quicker (and can be cached)