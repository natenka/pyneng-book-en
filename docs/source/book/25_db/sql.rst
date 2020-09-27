SQL
---

**SQL (structured query language)** - used to describe database structure, manage data (add, edit, delete, receive), manage access rights to the database and its objects, and manage transactions.

SQL language is divided into the following categories:

* DDL (Data Definition Language)
* DML (Data Manipulation Language)
* DCL (Data Control Language)
* TCL (Transaction Control Language)

Each category has its own operators (not all operators are listed):

* DDL 

  * CREATE - create new table, DBMS, schemas 
  * ALTER - change of existing table, columns 
  * DROP - removing existing objects from DBMS

* DML 

  * SELECT - data selection
  * INSERT - adding new data
  * UPDATE - updating existing data
  * DELETE - deleting data

* DCL 

  * GRANT - Allow users to read/write certain objects to DBMS
  * REVOKE - - withdrawal of prior authorizations

* TCL 

  * COMMIT - committing of transaction
  * ROLLBACK - rollback of all changes made in the current transaction

SQL and Python
^^^^^^^^^^^^

Two approaches can be used to work with a relational DBMS in Python:

* work with a library that corresponds to a specific database and use SQL language to work with the database. For example, sqlite uses sqlite3 module 
* work with `ORM <http://xgu.ru/wiki/ORM>`__ which uses an object-oriented approach to work with database. For example, Sqlalchemy
