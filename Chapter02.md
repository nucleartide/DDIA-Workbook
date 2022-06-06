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
