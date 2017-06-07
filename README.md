# Benefits of Using a Database-as-Code Strategy

### Getting the example up and running

* Start the database

I'm using docker here, as the example database is ephemeral

`docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=dbac_example -d -p 3306:3306 mysql`
 
`