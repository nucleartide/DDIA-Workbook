* Application developer data models: people, orgs, goods, actions, etc.
* Store data structures in general-purpose data model such as JSON, XML, relational tables, graph model
* Engineers that built the database represent data in terms of bytes on memory, disk, or network
* Hardware engineers represent bytes in electrical currents, light, magnets, etc.
* Relational model versus document model
    * Data organized into relations (tables)
    * Each relation is an unordered collection of tuples (rows)
    * Roots of relational databases lie in business data processing
    * Use cases: transaction processing (bank transactions, airline reservations, etc.) and batch processing (customer invoicing, payroll, reports)
    * Relational generalized very well beyond business data: online publishing, discussion, social networks, e-commerce, games, productivity apps, etc.
* NoSQL
    * Greater scalability, free and open source, specialized queries not supported by relational databases, more dynamic and expressive data model
    * Polyglot persistence: different databases have different use cases
* Impedance mismatch: translating between relational tables and application objects (this is a term borrowed from electronics)
    * ORM frameworks such as ActiveRecord and Hibernate
    * Traditional model: store one-to-many in separate tables
    * There are also structured data types now, such as XML and JSON
    * 3rd option is to store JSON strings, though you can’t leverage database querying capabilities this way
* Document DB such as Mongo, Rethink, CouchDB, and Espresso support document data model
    * Such as in resumes
    * JSON representation has better locality: in the multiple table example, you need multiple queries or a complex join query. When it’s 1 document, you only need 1 query
* Normalization: removing redundant copies of data
	* IDs never change: they have no meaning to humans
	* Work of making a join is shifted from database to application code
* Information Management System (IMS), developed for stockkeeping
* CODASYL network data model
* Relational model
	* Relation is a collection of rows
	* Query optimizer automatically decides which parts of the query to execute in what order, and which indexes to use
	* To query data in new ways, just declare a new index
* For one-to-many and many-to-many, relational and document databases are similar:
	* Foreign keys and document references are used
* Which is better?
	* Depends on whether your data has a document-like structure (all data is loaded at once) or not
* For highly interconnected data, the document model is awkward, the relational model is acceptable, and graph models are the most natural
* Schema-on-read versus schema-on-write (when is the schema enforced in document vs relational dbs)
* Schema changes have a bad rep, but it's an undeserved one
	* However MySQL copies the entire table on ALTER TABLE, which causes significant downtime
* Schemas are useful when all records are expected to have the same structure
* Need to load the entire document in a document DB, which affects performance in the case where documents are large. Thus, documents must be kept small
* Grouping data for locality is not limited to document databases
	* Examples: Google's Spanner, Oracle, Bigtable Column-oriented databases such as Cassandra and HBase
* Relational and document models are converging (Postgres and MySQL have json columns)
* Declarative query language hides implementation details of a database engine
	* Similar to React for the frontend world
* The Fact that SQL is unordered gives the database more room for automatic optimizations
* Declarative languages also take advantage of multi-core processors, in terms of executing in parallel, whereas imperative code demands running in sequence
* Imperative browser code: couples you to APIs, toggling styles is hard to maintain
* (stopped at mapreduce)
