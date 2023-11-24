# MongoDB Replica Set Setup on Windows

This guide provides a step-by-step process for setting up a MongoDB replica set on a Windows environment.

## Prerequisites

Ensure the following prerequisites are met before setting up the MongoDB replica set:

- Installed MongoDB on your Windows machine.
- Administrator access to modify file permissions.

## Step-by-Step Setup

### Step 1: Change File Permissions

To modify file permissions, follow these steps:

1. Open Command Prompt as an administrator.
2. Run the following command:

   ```shell
   icacls "C:\path\to\mongod.cfg" /grant "username":F
   ```

   Replace `"C:\path\to\mongod.cfg"` with your `mongod.cfg` file's actual path and `"username"` with your username.

### Step 2: Modify the Configuration File

Update the `mongod.cfg` file to enable replication by adding the following lines:

```yaml
replication:
  replSetName: rs0
```

### Step 3: Setting up MongoDB Replica Set using Docker

If you prefer Docker for setting up a MongoDB replica set, follow these steps:

1. Stop and remove existing Docker containers:

   ```shell
   docker-compose down
   ```

2. Update the `docker-compose.yaml` file to map ports correctly.

   Here is an example configuration:

   ```yaml
   version: '3'
   services:
     mongo1:
       image: mongo
       container_name: mongo1
       ports:
         - "3001:27017"
       command: mongod --replSet rs0
     mongo2:
       image: mongo
       container_name: mongo2
       ports:
         - "3002:27017"
       command: mongod --replSet rs0
     mongo3:
       image: mongo
       container_name: mongo3
       ports:
         - "3003:27017"
       command: mongod --replSet rs0
   ```

3. Start Docker containers:

   ```shell
   docker-compose up -d
   ```

4. Connect to the first MongoDB instance and initiate the replica set:

   ```shell
   docker exec -it mongo1 mongo
   rs.initiate({_id: "rs0", members: [{_id: 0, host: "mongo1:27017"}]})
   ```

5. Add the other two instances to the replica set:

   ```shell
   rs.add("mongo2:27017")
   rs.add("mongo3:27017")
   ```

6. Check the status of the replica set:

   ```shell
   rs.status()
   ```

## MongoDB Replica Set Commands and Operations

When working with a MongoDB replica set, various commands and operations help manage the set's configuration and status.

### Adding Members to the Replica Set

- **`rs.add()`**: Adds a new member to the replica set. For instance:
  ```javascript
  rs.add("newMember.example.com:27017")
  ```

- **`rs.addArb()`**: Adds an arbiter to the replica set, ensuring a voting member for elections. For example:
  ```javascript
  rs.addArb("arbiter.example.com:27017")
  ```

### Configuration and Information Retrieval

- **`rs.conf()`**: Retrieves the current replica set configuration details:
  ```javascript
  rs.conf()
  ```

- **`rs.status()`**: Provides the status of the replica set:
  ```javascript
  rs.status()
  ```

- **`rs.printReplicationInfo()`**: Displays detailed replication information from the primary's perspective:
  ```javascript
  rs.printReplicationInfo()
  ```

- **`rs.printSecondaryReplicationInfo()`**: Prints replication details from the secondary's perspective:
  ```javascript
  rs.printSecondaryReplicationInfo()
  ```

### Modifying Replica Set and Server Behavior

- **`rs.freeze()`**: Temporarily prevents the current member from seeking election as primary for a specified period:
  ```javascript
  rs.freeze(300) // Freezes for 300 seconds
  ```

- **`rs.reconfig()`**: Reconfigures the replica set by applying a new configuration object:
  ```javascript
  const newConfig = {
    _id: "rsNew",
    members: [
      { _id: 0, host: "mongo1:27017" },
      { _id: 1, host: "mongo2:27017" },
      // Add or remove members as needed
    ]
  }
  rs.reconfig(newConfig)
  ```

### Removing Members from the Replica Set

- **`rs.remove()`**: Removes a member from the replica set. For instance:
  ```javascript
  rs.remove("memberToRemove.example.com:27017")
  ```

### Converting Replica Set Member to Standalone Server

If you need to convert a replica set member to a standalone server, follow these steps:

1. Connect to the member you want to convert.
2. In the MongoDB shell, use the following command to step down and then shut down the member:
   ```javascript
   rs.stepDown()
   use admin
   db.shutdownServer()
   ```

3. Restart the server without the `--replSet` option.

For detailed instructions and considerations, refer to [this Stack Overflow post](https://stackoverflow.com/questions/16914281/how-to-convert-a-mongodb-replica-set-to-a-stand-alone-server).

### Handling Errors and Troubleshooting

- **Handling getaddrinfo ENOTFOUND Error**: If encountering a `getaddrinfo ENOTFOUND` error, ensure the provided hostname and port are correct and accessible. Refer to [this Stack Overflow post](https://stackoverflow.com/questions/39108992/mongoerror-getaddrinfo-enotfound-undefined-undefined27017) for troubleshooting steps.

Feel free to adjust these commands according to your specific use case and include them in your README for reference!

Source:
- [Stack Overflow - Convert MongoDB Replica Set to Standalone Server](https://stackoverflow.com/questions/16914281/how-to-convert-a-mongodb-replica-set-to-a-stand-alone-server)
- [Stack Overflow - getaddrinfo ENOTFOUND Error](https://stackoverflow.com/questions/39108992/mongoerror-getaddrinfo-enotfound-undefined-undefined27017)

For detailed information about these commands, refer to the [MongoDB Manual](^1^).

## Additional Resources

For further guidance and information, refer to the following resources:

- [MongoDB Manual - Replication Reference](https://www.mongodb.com/docs/manual/reference/replication/)
- [MongoDB Manual - Deploy a Replica Set](https://www.mongodb.com/docs/manual/tutorial/deploy-replica-set/)
- [IONOS - How to use a MongoDB Replica Set](https://www.ionos.com/digitalguide/websites/web-development/mongodb-replica-set/)
- [Guru99 - MongoDB Replication: How to Create MongoDB Replica Set](https://www.guru99.com/mongodb-replication.html)

If you encounter any issues or have questions, feel free to open an issue on this repository.

Ensure to replace placeholders with your actual values based on your setup.

Source: Conversation with Bing, 22/11/2023

(1) [Replication Reference — MongoDB Manual](https://www.mongodb.com/docs/manual/reference/replication/)
