# My MongoDB learning notes

## Install on Manjaro

MongoDB is currently available at the Arch User Repository (AUR) as ```mongodb-bin```, is was removed from the community repository because of licencing problems.

I also recommend the installation of compass and mongo tools, also available at AUR as ```mongodb-compass``` and ```mongodb-tools-bin``` respectively.


## Starting the database

The database can be started with the ```mongod``` command, however it will not work properly as this command expect a directory path to save the database. The command by default tries to use ```/data/db`` path, but it does not exist. Some extra arguments must be set to the command to work.

```shell
# I'm putting the data in a path inside my project folder but it can be anywhere
cd <project-folder>
mkdir ./data/
mongod --dbpath "$(pwd)/data/"

# We can also the default stdout log by redirecting it to a file
mongod --dbpath "$(pwd)/data/" --logpath "$(pwd)/mongo.log" --logappend
```