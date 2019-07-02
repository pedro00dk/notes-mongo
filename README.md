# My MongoDB learning notes

MongoDB is a no-sql database software which uses a document oriented structure. 

References:
* https://docs.mongodb.com/manual/reference/
* https://docs.mongodb.com/manual/reference/method/
* https://www.youtube.com/watch?v=pWbMrx5rVBE

## Install on Manjaro

MongoDB is currently available at the Arch User Repository (AUR) as `mongodb-bin`, is was removed from the community repository because of licencing problems.

I also recommend the installation of compass and mongo tools, also available at AUR as `mongodb-compass` and `mongodb-tools-bin` respectively.


## Starting the database

The database can be started with the `mongod` command, however it will not work properly as this command expect a directory path to save the database. The command by default tries to use ```/data/db`` path, but it does not exist. Some extra arguments must be set to the command to work.
```shell
# I'm putting the data in a path inside my project folder but it can be anywhere
cd <project-folder>
mkdir ./data/
mongod --dbpath "$(pwd)/data/"

# We can also the default stdout log by redirecting it to a file
mongod --dbpath "$(pwd)/data/" --logpath "$(pwd)/mongo.log" --logappend
```

This command has several extra options that can be accessed through `mongod --help`

## Shell

After starting the the mongo daemon, we can run the `mongo` command to access the mongo shell.

By default the mongo will put the user in a `test` database. To check the current selected database we can use the command `db` inside the shell.

To list the databases and its current sizes, we can use the following commands:
```shell
show databases
show dbs
```

It is possible to create a new database and switch to it using the `use` command:
```javascript
// create a new database and switch to it
// use <database-name>
use data
```

#### Creating database users

To create a new user, the `db.createUser(<user-obj>)` has to be used, the user must pass its login, password and an array of role objects that can be simplified to strings.
```javascript
db.createUser({
    user: 'pedro',
    pwd: '1234',
    roles: ['readWrite', 'dbAdmin']
})
```

## CRUD

#### Create

MongoDb stores its data in collections, they are like tables in relational databases. To create a new collection, we use the following command:
```javascript
db.createCollection('customers')
```

We can list the the collections of the current database by running `show collections`.

Before making queries, we must have some data in our database. MongoDB use a (js-json)-like syntax for creating and querying its documents.
```javascript
// db.<collection>.insert(<document> | <document>[])
// if more than one document is passed as parameter, it is ignored
// to insert more than one document, they must be passed in an array of documents
db.customers.insert({name: 'John', surname: 'Doe'})
db.customers.insert([{name: 'Steven', surname: 'Smith'}, {name: 'Joan', surname: 'Johnson', gender: 'female'}])

// insertOne and insertMany are the supported variations of the insert function.
db.customers.insertOne({name: 'John', surname: 'Doe'})
db.customers.insertMany([{name: 'Steven', surname: 'Smith'}, {name: 'Joan', surname: 'Johnson', gender: 'female'}])
```

Documents do not need to have the same fields, but having a logical. For example, the last document in the insert many function has an extra `gender` field.

#### Read

The main function to read data from a collection is `find`, this function without arguments will return all documents from a collection. This function can receive filter arguments, and also has some variations, such as `fidOne`.
For better reading purposes, we can chain the `.pretty` function to pretty print the output.
```javascript
// db.<collection>.find(<query>?, <projection>?)
db.customers.find()
db.customers.find().pretty()

// The output can also be stored in variables for posterior processing
let result = db.customers.find()

// const result = ...
// var result = ...
```

#### Update

Collection documents can be updated though the update function. 

#### Delete