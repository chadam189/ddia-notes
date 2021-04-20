#### Part 1 Review - Time Series DB Analysis

#### Time Series DB Requirements

Billions of individual data points
High write throughput
High read throughput
Large deletes to free up disk space
Mostly an insert/append workload, very few updates

#### Progression through various DB engines underneath Influx

As we learned more about what people needed with time series data, we encountered a few insurmountable challenges with our intial implementation

*First:* LevelDB is an implementation of a Log Structured Merge Tree (or LSM Tree) - opensource Google DB
   - The two biggest advantages that LevelDB had for us were high write throughput and built in compression
   - *With LSMTrees inside LevelDB, the cost of deleting data is too high*
   - *LevelDB only supports cold backup*
   - Storing large amounts of data in the standalone environment cannot occupy too any handles.
   - *In LevelDB, lots of small files will be generated over time*

*Second:* BoltDB, a pure Golang database (another key/value store)
   - *The B+tree write operation inside BoltDB has the throughput bottleneck*
   - *BoltDB does not support compression*
   - Rocks DB not written in Go, they want both the DBMS AND the storage engine to be written in go
   - WAL to "batch" writes to the index - the homegrown WAL was lighting fast but the index itself couldn't keep up

*Finally:* WAL + TSM file-based scheme

#### Problems they found along the way

1. hot backups (to make a backup you have to close DB => copy it => open back up) - 

1. data retention - 
   1. you need high precision data for a short time, then you can scrap it (after you aggreagate and "downsample" it into condensed data sets for historical recordkeeping)
   1. you're gonna do a lot of deletes - let's say your retention time is 7 days, you write 100 logs a day. on day 7 you have 700 logs. on day 8 you'll have 100 writes and 100 deletes, so you'll still end up with 700 logs at the end of day 8

1.  large deletions are very expensive 
   1. To get around doing deletes, we split data across what we call shards, which are contiguous blocks of time. Shards would typically hold either a day or 7 days worth of data. Each shard mapped to an underlying LevelDB. This meant that we could drop an entire day of data by just closing out the database and removing the underlying files.


#### Links

https://medium.com/dataseries/analysis-of-the-storage-mechanism-in-influxdb-b84d686f3697
https://archive.docs.influxdata.com/influxdb/v0.12/concepts/storage_engine/
