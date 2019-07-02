# My MongoDB learning notes

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
