### Questions

#### Chapter 1

1. What is a search index? How does it differ from a cache or DB? Is Elasticsearch one? 

1. Stream and batch processing - examples of each? (Logs for stream, ??? for batch)

#### Chapter 2

1. run "EXPLAIN ANALYZE" for PostgreSQL queries

1. Learn what a materialized view is (vs a view - is that same as stored procedure?)
    - https://www.compose.com/articles/its-a-view-its-a-table-no-its-a-materialized-view/

1. What is the "STATSD" table in PG?

1. How does ALTER TABLE work in MySQL vs PG vs other? Why does ALTER TABLE

1. Data locality - how is memory pulled from disk? Is it in one chunk? Does DL happen in HDD vs SSD?

1. What is a query planner (vs query optimizer)?

#### Chapter 3

1. Difference in DB storage engines?
    - https://stackoverflow.com/questions/3818759/what-is-innodb-and-myisam-in-mysql

1. Do most SQL DB's automatically index primary keys? What about foreign keys - when you declare a foreign key they'll 1) index it and 2) verify it's a valid value

1. Are time-series DB in column-stores? What makes them more geared towards time-series querying over PostgreSQL? Is a time-series DB better, period? 




#### Group Discussion

Jonahthan - indexing on boolean columns (aka cols with only a handful of distinct vals)
Beth - researching how data is pulled from disk on HDD vs SSD - is it by a single size of block of memory (always a 4kb chunk)? what takes the most time, is it finding the block, or moving the needle? or is it seeking the exact address? or is it copying the data to memory once it's located?
Chadam - time series DBs....what makes them special?
