# MongoDB Replica Set Setup on Windows

This guide will walk you through the process of setting up a MongoDB replica set on Windows.

## Prerequisites

- MongoDB installed on your Windows machine.
- Administrator access to change file permissions.

## Step 1: Change File Permissions

First, we need to change the read, write, and execute privileges of the `mongod.cfg` file. Open the command prompt as an administrator and run the following command:

```shell
icacls "C:\path\to\mongod.cfg" /grant "username":F
```

Here, replace `"C:\path\to\mongod.cfg"` with the actual path to your `mongod.cfg` file and `"username"` with your actual username.

The permissions abbreviations are as follows:

- F: Full control
- M: Modify access
- RX: Read and execute access
- R: Read-only access
- W: Write-only access

## Step 2: Modify the Configuration File

Next, we need to modify the `mongod.cfg` file to enable replication. Add the following lines to the file:

```yaml
replication:
  replSetName: rs0
```
![](https://res.cloudinary.com/dj76d2css/image/upload/v1700647533/MKevyji9Ef_idbfvj.png)

## Step 3: Start MongoDB with the Replica Set Option

Open a new command prompt and start MongoDB with the `--replSet` option:

```shell
mongod --replSet rs0
```
![](https://res.cloudinary.com/dj76d2css/image/upload/v1700647546/WindowsTerminal_QtzsmbEk98_qwtzuu.png)

## Step 4: Connect to MongoDB Shell

In a new command prompt window, connect to the MongoDB shell by typing:

```shell
mongosh
```

## Step 5: Initiate the Replica Set

Finally, initiate the replica set using the following command in the MongoDB shell:

```shell
rs.initiate()
```
![](https://res.cloudinary.com/dj76d2css/image/upload/v1700647553/WindowsTerminal_jbxO3Ke9Cf_u33ziu.png)

## MongoDB Replica Set Commands

Here are some commonly used MongoDB replica set commands¹:

- `rs.add()`: Adds a member to a replica set.
- `rs.addArb()`: Adds an arbiter to a replica set.
- `rs.conf()`: Returns the replica set configuration document.
- `rs.freeze()`: Prevents the current member from seeking election as primary for a period of time.
- `rs.help()`: Returns basic help text for replica set functions.
- `rs.initiate()`: Initializes a new replica set.
- `rs.printReplicationInfo()`: Prints a formatted report of the replica set status from the perspective of the primary.
- `rs.printSecondaryReplicationInfo()`: Prints a formatted report of the replica set status from the perspective of the secondaries.
- `rs.reconfig()`: Re-configures a replica set by applying a new replica set configuration object.
- `rs.remove()`: Remove a member from a replica set.
- `rs.status()`: Returns a document with information about the state of the replica set.
- `rs.stepDown()`: Causes the current primary to become a secondary which forces an election.
- `rs.syncFrom()`: Sets the member that this replica set member will sync from, overriding the default sync target selection logic.

For more detailed information about these commands, please refer to the [MongoDB Manual](^1^).

That's it! You have successfully set up a MongoDB replica set on your Windows machine. If you have any questions or run into any issues, feel free to open an issue on this repository.

Please replace the placeholders with the actual values as per your setup. Let me know if you need any further assistance!

Source: Conversation with Bing, 22/11/2023
(1) Replication Reference — MongoDB Manual. https://www.mongodb.com/docs/manual/reference/replication/.
(2) Replication Reference — MongoDB Manual. https://www.mongodb.com/docs/manual/reference/replication/.
(3) Deploy a Replica Set — MongoDB Manual. https://www.mongodb.com/docs/manual/tutorial/deploy-replica-set/.
(4) How to use a MongoDB Replica Set - IONOS. https://www.ionos.com/digitalguide/websites/web-development/mongodb-replica-set/.
(5) MongoDB Replication: How to Create MongoDB Replica Set - Guru99. https://www.guru99.com/mongodb-replication.html.
