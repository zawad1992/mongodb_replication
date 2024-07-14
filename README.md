# MongoDB Replication Setup

This guide will walk you through the process of setting up a MongoDB replica set on both Linux and Windows. This includes installation, configuration, and starting MongoDB instances.

## Prerequisites

- MongoDB installed on your machine.
- MongoDB Shell (`mongosh`) for interacting with MongoDB instances.
- For Windows, ensure MongoDB binaries are added to the system's PATH.

## Installation

### Install MongoDB

#### On Linux 

1. For Debian based OS (eg. Ubuntu) please follow the installation procedure from official website [install-mongodb-on-ubuntu](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/).
2. For Fedora based OS like (eg. Red Hat or CentOS) please follow the installation procedure from official website [install-mongodb-on-red-hat](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/).


#### On Windows

1. Download the MongoDB installer from the [official website](https://www.mongodb.com/try/download/community).
2. Run the installer and follow the installation instructions.
3. Ensure the MongoDB binaries (`mongod`, `mongo`, `mongosh`) are added to your system's PATH.

### Install MongoDB Shell

1. Download MongoDB Shell (`mongosh`) from the [official website](https://www.mongodb.com/try/download/shell).
2. Place `mongosh.exe` in the MongoDB installation directory (`<MongoDB-Install-Directory>\Server\7.0\bin`).

## Setting Up Replica Set

### Directory Structure

Ensure you have the following directories created:

#### On Linux

```sh
sudo mkdir -p /var/lib/mongodb/db1
sudo mkdir -p /var/lib/mongodb/db2
sudo mkdir -p /var/lib/mongodb/db3
```

#### On Windows

```plaintext
D:\SERVER\MongoDB\Data\db1
D:\SERVER\MongoDB\Data\db2
D:\SERVER\MongoDB\Data\db3
```

### Starting MongoDB Instances

#### On Linux

```sh
sudo mongod --port 27018 --bind_ip_all --dbpath /var/lib/mongodb/db1 --replSet rs0
sudo mongod --port 27019 --bind_ip_all --dbpath /var/lib/mongodb/db2 --replSet rs0
sudo mongod --port 27020 --bind_ip_all --dbpath /var/lib/mongodb/db3 --replSet rs0
```

#### On Windows

```sh
mongod --port 27018 --bind_ip_all --dbpath D:\SERVER\MongoDB\Data\db1 --replSet rs0
mongod --port 27019 --bind_ip_all --dbpath D:\SERVER\MongoDB\Data\db2 --replSet rs0
mongod --port 27020 --bind_ip_all --dbpath D:\SERVER\MongoDB\Data\db3 --replSet rs0
```

### Connecting to MongoDB Instances

#### On Linux

```sh
mongo --port 27018
mongo --port 27019
mongo --port 27020
```

#### On Windows

```sh
mongosh --port 27018
mongosh --port 27019
mongosh --port 27020
```

### Initialize Replica Set

1. Connect to the primary instance (`27018`).
#### For Linux
```sh
mongo --port 27018
```
#### For Windows
```sh
mongosh --port 27018
```

2. Run the following commands:

```javascript
rs.initiate({
    _id: "rs0",
    members: [
        { _id: 0, host: "127.0.0.1:27018" },
        { _id: 1, host: "127.0.0.1:27019" },
        { _id: 2, host: "127.0.0.1:27020" }
    ]
})

```
##### You can set remote IP on host if you create replica set in different server but make sure to check and allow firewall for the specific port (27018,27019,27020)

3. Check the replica set status:

```javascript
rs.status();
```

### Setting Read Preferences

To allow reading from secondary nodes:

```javascript
use admin
db.getMongo().setReadPref("primaryPreferred");
```

## Connection Strings

Use the following connection strings to connect to your MongoDB replica set:

```plaintext
mongodb://127.0.0.1:27018/?replicaSet=rs0&directConnection=true
mongodb://127.0.0.1:27019/?replicaSet=rs0&directConnection=true
mongodb://127.0.0.1:27020/?replicaSet=rs0&directConnection=true
```

##### Remember this is basic replication system and not authentication system provided. If you need authentication and IP binding I'll provide this instruction soon. Thank you. ######
