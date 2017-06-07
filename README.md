# Benefits of Using a Database-as-Code Strategy

### Getting the example up and running

* Start the database

I'm using docker here, as the example database is ephemeral

`docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=dbac_example -d -p 3306:3306 mysql`

### Tooling

This example uses [Liquibase](http://www.liquibase.org/) to achieve a database as code.  Liquibase 
works well with a variety of databases including MySQL, PostgreSQL, Oracle, and DB2 - 
see the [full list of supported databases](http://www.liquibase.org/databases.html).
 
### Benefits of using the Database-as-Code Strategy

* Databases can be built with the click of a button
  * This supports Continuous Integration (CI) and Continuous Delivery (CD) strategies
* Every developer can run their own copy of the database locally (WHAT!?!?!?)
* Deploying database updates is easy
* Simplified testing
* Database schema is checked into source control right alongside application source code
* Auditable database changelog
  * Team members can review changes and see when changes were made and by whom
 
`