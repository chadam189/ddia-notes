## Chapter 2 - Data Models and Query Languages

### Overview

1. Relational Model vs Operational Model vs Graph Model
1. Query Languages

### Key Concept

1. Three most important questions to ask when choosing a database:

* What format/shape the data is in?
* What is the access pattern we're optimizing for? ("reads of a user's twitter timeline", "writing logs", "reading/writing session data", "describing the connections of a user in a social network")
* Will I need to run joined queries against this data? (Kinda a part of 2 but super important so worth re-iterating)

1. Historically, data was stored in tree format (heirarchical model). We quickly realized it's hard as hell to manage many-to-many relationships in that model, and to query stuff. So we invented the relational model to solve that problem.

1. Document databases target use cases where data comes in self-contained documents, and relationships between one document and another are rare

1. Graph databases target use cases where anything is potentially related to anything

1. Applications always assume data has a structure - it just depends whether it's explicit (enforced on write => relational) or assumed on read (implicit => document, graph)

### Terms/Definitions

* relation => what a table is called in an RDBMS (SQL) database
* tuple => what an unordered row is called in the table of an RDBMS (SQL) database

### Phrases I Should Use

* "access patterns" => db, cache, message queue are all data systems, technically...but they have very different access patterns (aka user requirements) which means different performance characteristic 
* "primary key" => ID of a tuple (row) in a SQL relation (table)
* "foreign key" => ID of a tuple (row) from ANOTHER relation (table)
* "one-to-many" => college to students, teacher 
* "many-to-one" => students to college, classes to teacher
* "many-to-many" => students to classes, classes to students
* "normalized" => data is only stored in a single place in the DB
* "denormalized" => data is copied when it COULD be stored in just one place

* "schema-on-read" => store data as soon as we recive it, and worry about validation/analysis later
* "schema-on-write" => define columns, data format, relationships between tables BEFORE we write to the DB, validate data, and only then can we upload. but queries are quick!!
* "document reference" =>
* "data locality" => taking advantage of "data locality" by using a document store DB when we want to optimize for pulling everything about a particular user (instead of having to do a bunch of index lookups across multiple tables, which is what we'd have to do for this same data stored in a traditional relational schema)
* "imperative" => you give the computer instructions and it comes up with the result
* "declartive" => you give the computer a result and it comes up with the instructions
* "query optimizer" => 


### NoSQL Data Stores

1. Key-Value stores (Redis, DynamoDB from AWS)

"Keys" are the unique identifiers of each entry

* Fast reads, fast writes
* Does NOT know anything about the value stored in it, therefore very little ability to query or do joins
* Best used for caches

1. Document stores (MongoDB)

### Schema-on-write

Penalty is paid UP FRONT during writes, because we apply schema first

Not flexible, very structured, works best when querying is a key access pattern and we know how data will be formatted

Data is made for a SPECIFIC LIMITED PURPOSE, cannot be reused for future uses we do not know yet

SOW: Receive Data => Apply Schema => Store Data => Read Data (Analyze/Query)

### Schema-on-read

Best used when fast ingestion (writes) are needed, or if schema changes are expected, or if we just want to store data without much concern for its structure

Error-prone: we don't check for duplicates, missing fields, invalid fields

SOR: Receive Data => Store Data => Read Data (Analyze/Query) => Apply Schema

### Links

- https://seldo.com/posts/databases_how_they_work_and_a_brief_history
   - https://twitter.com/simonleggsays/status/1196480645234597890
- https://seldo.com/posts/in_defence_of_sql
- https://www.javatpoint.com/dbms-reduction-of-er-diagram-into-table
- https://luminousmen.com/post/schema-on-read-vs-schema-on-write
- https://www.quora.com/What-are-the-main-differences-between-the-four-types-of-NoSql-databases-KeyValue-Store-Column-Oriented-Store-Document-Oriented-Graph-Database