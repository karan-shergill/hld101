# Chapter 2: Data Models and Query Languages

- [Relational Model Versus Document Model](#relational-model-versus-document-model)
  - [The Birth of NoSQL](#the-birth-of-nosql)
  - [The Object-Relational Mismatch](#the-object-relational-mismatch)
  - [Many-to-One and Many-to-Many Relationships](#many-to-one-and-many-to-many-relationships)
  - [Are Document Databases Repeating History?](#are-document-databases-repeating-history)
    - [The relational model](#the-relational-model)
    - [Comparison to document databases](#comparison-to-document-databases)
  - [Relational Versus Document Databases Today](#relational-versus-document-databases-today)
    - [Which data model leads to simpler application code?](#which-data-model-leads-to-simpler-application-code)
    - [Schema flexibility in the document model](#schema-flexibility-in-the-document-model)
    - [Data locality for queries](#data-locality-for-queries)
  - [Query Languages for Data](#query-languages-for-data)
  - [MapReduce Querying](#mapreduce-querying)
- [Graph-Like Data Models](#graph-like-data-models)
  - [Property Graphs](#property-graphs)
  - [Graph Queries in SQL](#graph-queries-in-sql)
  - [Triple-Stores and SPARQL](#triple-stores-and-sparql)
  - [The Foundation: Datalog](#the-foundation-datalog)

* Data models are the most important part of developing software, because they affect not only how the software is written, but how we think about the problem that we're solving.
* Layered software abstractions allow different groups of people to work together effectively.

### Relational Model Versus Document Model

#### The Birth of NoSQL

* The term NoSQL has been retroactively reinterpreted as _Not Only SQL_.

#### The Object-Relational Mismatch

* Impedance mismatch is the disconnect between objects in the application code and the database model of tables, rows, and columns.
* A JSON representation of a document-oriented database has better locality than an equivalent multi-table schema.

#### Many-to-One and Many-to-Many Relationships

* In a document-oriented database, if information is duplicated and that information is changed, then all redundant copies need to be updated. Normalization is the key idea behind removing such duplication.

#### Are Document Databases Repeating History?

##### The relational model

* A key insight of the relational model was that you only need to build a query optimizer once, and then all database clients can benefit from it.

##### Comparison to document databases

* For many-to-one and many-to-many relationships, a related item is referenced by a *foreign key* in the relational model, and a *document reference* in the document model.

#### Relational Versus Document Databases Today

* The document data model offers greater schema flexibility, better performance from locality, and for some applications it is closer to the data structures used by the application.
* The relational model offers better support for joins, and many-to-one and many-to-many relationships.

##### Which data model leads to simpler application code?

* If your application data has a document-like structure (i.e. a tree of one-to-many relationships, where typically the entire tree is loaded at once), then a document model is likely a good idea.
* But if your application uses many-to-many relationships, the document model becomes less appealing.

##### Schema flexibility in the document model

* Document databases are sometimes called *schemaless*, but that's misleading – there is an implicit schema assumed by the application, but not enforced by the database.
* A more accurate term is *schema-on-read* in contrast with *schema-on-write*.
* Schema-on-read is similar to dynamic (runtime) type checking in programming languages, whereas schema-on-write is similar to static (compile-type) type checking.
* In cases where all records are expected to have the same structure, schemas are a useful mechanism for documenting and enforcing a data structure.

##### Data locality for queries

* The locality advantage only applies if you need large parts of the document at the same time.

#### Query Languages for Data

* An imperative language tells the computer to perform certain operations in a certain order.
* In a declarative query language like SQL or relational algebra, you specify the pattern of the data you want, but not *how* to achieve that goal.
* A declarative query language also hides implementation details of the database engine, allowing it to add performance improvements without requiring changes to queries.
* Declarative languages are more amenable to parallel execution because they specify the pattern of the results, but not the algorithm used to determine them.

#### MapReduce Querying

* MapReduce is based on a `map` (or `collect`) and `reduce` (or `fold` or `inject`) functions that exist in many functional languages.
* The `map` and `reduce` functions must be pure, thereby allowing the database to run them anywhere, and to rerun them on failure.

### Graph-Like Data Models

#### Property Graphs

* A property graph model, each vertex and edge has a collection of properties (i.e. key-value pairs), and each edge has a label to describe the relationship between its vertices.
* To get the set of incoming and outgoing edges for a vertex, you can query the `edges` table by `head_vertex` or `tail_vertex` respectively.

#### Graph Queries in SQL

* In a relational database, you usually know in advance which joins you need in your query.
* In a graph query, you may need to traverse a variable number of edges before you find the vertex you're looking for – i.e. the number of joins is not fixed in advance.

#### Triple-Stores and SPARQL

* In a triple-store, all information is stored in the form of very simple three-part statements: (_subject_, _predicate_, _object_).
* The object is one of two things:
  * A value in a primitive data type, and so the predicate and object are equivalent to the key and value of a property on the subject vertex. (E.g. (_lucy_, _age_, _33_) is like a vertex `lucy` with properties `{"age": 33}`.)
  * Another vertex in the graph, and so the predicate is an edge, the subject is the tail vertex, and the object is the head vertex. (E.g. (_alice_, _married_to_, _bob_) has an edge _married_to_ between _alice_ and _bob_.)

#### The Foundation: Datalog

* Datalog generalizes the triple-store model, writing the triple as _predicate(subject, object)_.
* A Datalog rule applies if the system can find a match for _all_ predicates on the right side of the `:-` operator.
* When a rule applies, it's as though the left side of the `:-` was added to the database, with variables replaced by the values they matched.
