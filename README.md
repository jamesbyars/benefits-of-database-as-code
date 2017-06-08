# Benefits of Using a Database-as-Code Strategy

### Getting the example up and running

1. Start the database

    * `docker run -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=dbac_example -d -p 3306:3306 mysql`
        * I'm using docker here, as the example database is ephemeral, you can use any 
        database and update the connection string in the `application.properties` file
        * This is not secure, it is meant to be a simple example and should not be used 
        in production
    
2. Install Dependencies (Build using Java 8 and Apache Maven 3.3.9)

    * `mvn install`
    
3. Run the `DatabaseAsCodeApplication` class 

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
  
### How does it work?

Liquibase runs each time the application starts up.  If you open `application.properties` 
you will notice it contains a property `liquibase.change-log`.  This property tells Liquibase
where to look for the changelogs.

On each run Liquibase calculates a Hash for each changeset.  It compares the Hash with those found 
in a table managed by Liquibase called DATABASECHANGELOG.  If the Hash matches, Liquibase assumes 
the change has been run and skips that changeset.  If the Hash does not match Liquibase executes 
the changeset and inserts the Hash into the DATABASECHANGELOG table.
  
## DB Change Examples

### Adding a column to an existing table in the database

#### Incorrect - Running an alter statement agains the DB

`ALTER TABLE user ADD COLUMN preferred_name text;`
    
#### Correct - Adding a changeset to source control

    <changeSet id="add-preferred-name-to-user-table" author="jbyars">
    
        <addColumn tableName="user">
            <column name="preferred_name" type="text"/>
        </addColumn>
        
    </changeSet>
     
#### Explanation

The incorrect method is 'run once and forgotten', there  is no simple way to automatically run, or validate that the change has already run, in the 
next environment.

The correct solution is database agnostic, documents the change, and is committed to source 
control.