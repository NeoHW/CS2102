## Tutorial 1 Notes

### Insert 0 1

what does Insert 1 0 mean ?

- "Insert": Indicates that an INSERT operation was performed.
- "1": The number of rows successfully inserted.
- "0": The number of errors or warnings encountered during the operation.

### Update 21

- "Update": Indicates that an UPDATE operation was executed.
- "21": The number "21" typically represents the number of rows that were affected by the update operation. In this case, it means that 21 rows were updated by the executed command.



### ON DELETE CASCADE / ON UPDATE CASCADE

This tells Postgres to automatically delete any rows in the referenced table that are related to the row being deleted in the referencing table.



### Unique Constraint

By default, two null values are not considered equal in this comparison. That means even in the presence of a unique constraint it is possible to store duplicate rows that contain a null value in at least one of the constrained columns.



### DDL, DML, DQL, DCL

Data Definition Language (DDL) for structuring data(

​	create table,

​	alter table,

​	drop table

**Data Manipulation Language (DML)** -

`INSERT`, `UPDATE`, and `DELETE`

**Data Query Language (DQL)** 

`Select`

**Data Control Language (DCL)** 

administrative tasks of controlling the database itself

GRANT, REVOKE, and DENY



### DEFERRABLE

support : UNIQUE, PRIMARY KEY, EXCLUDE, and REFERENCES (foreign key) constraints accept this clause

**NOT DEFERRABLE**(default, A constraint that is not deferrable will be checked immediately after every command)

**DEFFERABLE**

- INITIALLY IMMEDIATE: checked after each statement
- INITIALLY DEFERRED: checked only at the end of the transaction



Read the post if you want to know more about difference between NOT DEFERRABLE VS DEFFERABLE INITIALLY IMMEDIATE

https://stackoverflow.com/questions/5300307/not-deferrable-versus-deferrable-initially-immediate