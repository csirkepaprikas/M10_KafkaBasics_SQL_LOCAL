# Module 1:  Kafka Basics
### Balázs Mikes

#### github link:
https://github.com/csirkepaprikas/M08_SparkML_PYTHON_AZURE.git
## This module is dedicated to Kafka.

This task is a basic introduction to the Kafka world, and it will guided me through basic operations on top of Kafka.
This assignment actively used topics from Kafka Basic and Kafka Streams, as Kafka is one of the most commonly used applications in streaming contexts today.
Throughout the task, I followed basic tutorials and implemented them.
By the end of the assignment, I got more familiar with the basic operations in Kafka.


## Preparation:
For the proper usage of Kafka I installed a WSL on my computer and also installed there open JDK 21
## Step 1: Get Kafka
Downloaded the latest Kafka release and extract it: 

```python
bmikes@bmikes:~$ wget https://downloads.apache.org/kafka/4.0.0/kafka_2.13-4.0.0.tgz
--2025-04-02 16:57:49--  https://downloads.apache.org/kafka/4.0.0/kafka_2.13-4.0.0.tgz
Resolving downloads.apache.org (downloads.apache.org)... 88.99.208.237, 135.181.214.104, 2a01:4f9:3a:2c57::2, ...
Connecting to downloads.apache.org (downloads.apache.org)|88.99.208.237|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 132045169 (126M) [application/x-gzip]
Saving to: ‘kafka_2.13-4.0.0.tgz’

kafka_2.13-4.0.0.tgz                                 100%[===================================================================================================================>] 125.93M  6.58MB/s    in 22s

2025-04-02 16:58:09 (5.73 MB/s) - ‘kafka_2.13-4.0.0.tgz’ saved [132045169/132045169]

bmikes@bmikes:~$
bmikes@bmikes:~$ ll
total 128976
drwxr-x--- 3 bmikes bmikes      4096 Apr  2 16:57 ./
drwxr-xr-x 3 root   root        4096 Apr  2 16:49 ../
-rw-r--r-- 1 bmikes bmikes       220 Apr  2 16:49 .bash_logout
-rw-r--r-- 1 bmikes bmikes      3771 Apr  2 16:49 .bashrc
drwx------ 2 bmikes bmikes      4096 Apr  2 16:50 .cache/
-rw-r--r-- 1 bmikes bmikes         0 Apr  2 16:50 .motd_shown
-rw-r--r-- 1 bmikes bmikes       807 Apr  2 16:49 .profile
-rw-r--r-- 1 bmikes bmikes         0 Apr  2 16:51 .sudo_as_admin_successful
-rw-r--r-- 1 bmikes bmikes 132045169 Mar 18 09:29 kafka_2.13-4.0.0.tgz
bmikes@bmikes:~$ tar -xzf kafka_2.13-4.0.0.tgz
bmikes@bmikes:~$ ll
total 128980
drwxr-x--- 4 bmikes bmikes      4096 Apr  2 16:58 ./
drwxr-xr-x 3 root   root        4096 Apr  2 16:49 ../
-rw-r--r-- 1 bmikes bmikes       220 Apr  2 16:49 .bash_logout
-rw-r--r-- 1 bmikes bmikes      3771 Apr  2 16:49 .bashrc
drwx------ 2 bmikes bmikes      4096 Apr  2 16:50 .cache/
-rw-r--r-- 1 bmikes bmikes         0 Apr  2 16:50 .motd_shown
-rw-r--r-- 1 bmikes bmikes       807 Apr  2 16:49 .profile
-rw-r--r-- 1 bmikes bmikes         0 Apr  2 16:51 .sudo_as_admin_successful
drwxr-xr-x 7 bmikes bmikes      4096 Mar 14 09:20 kafka_2.13-4.0.0/
-rw-r--r-- 1 bmikes bmikes 132045169 Mar 18 09:29 kafka_2.13-4.0.0.tgz
bmikes@bmikes:~$ cd kafka_2.13-4.0.0/
```

## Step 2: Start the Kafka environment

First I generated a Cluster UUID, then formatted Log Directories and  started the Kafka Server:
```python
bmikes@bmikes:~/kafka_2.13-4.0.0$ KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
bmikes@bmikes:~/kafka_2.13-4.0.0$ bin/kafka-storage.sh format --standalone -t $KAFKA_CLUSTER_ID -c config/server.properties
Formatting dynamic metadata voter directory /tmp/kraft-combined-logs with metadata.version 4.0-IV3.
bmikes@bmikes:~/kafka_2.13-4.0.0$ bin/kafka-server-start.sh config/server.properties
[2025-04-02 16:59:07,436] INFO Registered kafka:type=kafka.Log4jController MBean (kafka.utils.Log4jControllerRegistration$)
[2025-04-02 16:59:07,876] INFO Registered signal handlers for TERM, INT, HUP (org.apache.kafka.common.utils.LoggingSignalHandler)
[2025-04-02 16:59:07,881] INFO [ControllerServer id=1] Starting controller (kafka.server.ControllerServer)
[2025-04-02 16:59:08,307] INFO Updated connection-accept-rate max connection creation rate to 2147483647 (kafka.network.ConnectionQuotas)
[2025-04-02 16:59:08,377] INFO [SocketServer listenerType=CONTROLLER, nodeId=1] Created data-plane acceptor and processors for endpoint : ListenerName(CONTROLLER) (kafka.network.SocketServer)
[2025-04-02 16:59:08,398] INFO authorizerStart completed for endpoint CONTROLLER. Endpoint is now READY. (org.apache.kafka.server.network.EndpointReadyFutures)
[2025-04-02 16:59:08,400] INFO [SharedServer id=1] Starting SharedServer (kafka.server.SharedServer)
[2025-04-02 16:59:08,511] INFO [LogLoader partition=__cluster_metadata-0, dir=/tmp/kraft-combined-logs] Loading producer state till offset 0 (org.apache.kafka.storage.internals.log.UnifiedLog)
[2025-04-02 16:59:08,513] INFO [LogLoader partition=__cluster_metadata-0, dir=/tmp/kraft-combined-logs] Reloading from producer snapshot and rebuilding producer state from offset 0 (org.apache.kafka.storage.internals.log.UnifiedLog)
[2025-04-02 16:59:08,529] INFO [LogLoader partition=__cluster_metadata-0, dir=/tmp/kraft-combined-logs] Producer state recovery took 1ms for snapshot load and 0ms for segment recovery from offset 0 (org.apache.kafka.storage.internals.log.UnifiedLog)
[2025-04-02 16:59:08,590] INFO Initialized snapshots with IDs SortedSet(OffsetAndEpoch(offset=0, epoch=0)) from /tmp/kraft-combined-logs/__cluster_metadata-0 (kafka.raft.KafkaMetadataLog$)
[2025-04-02 16:59:08,615] INFO [raft-expiration-reaper]: Starting (kafka.raft.TimingWheelExpirationService$ExpiredOperationReaper)
[2025-04-02 16:59:08,624] INFO [RaftManager id=1] Starting request manager with bootstrap servers: [localhost:9093 (id: -2 rack: null isFenced: false)] (org.apache.kafka.raft.KafkaRaftClient)
[2025-04-02 16:59:08,632] INFO [RaftManager id=1] Reading KRaft snapshot and log as part of the initialization (org.apache.kafka.raft.KafkaRaftClient)
[2025-04-02 16:59:08,637] INFO [RaftManager id=1] Loading snapshot (OffsetAndEpoch(offset=0, epoch=0)) since log start offset (0) is greater than the internal listener's next offset (-1) (org.apache.kafka.raft.internals.KRaftControlRecordStateMachine)
[2025-04-02 16:59:08,654] INFO [RaftManager id=1] Latest kraft.version is KRAFT_VERSION_1 at offset -1 (org.apache.kafka.raft.internals.KRaftControlRecordStateMachine)
[2025-04-02 16:59:08,656] INFO [RaftManager id=1] Latest set of voters is VoterSet(voters={1=VoterNode(voterKey=ReplicaKey(id=1, directoryId=F7htgtxrSBykCD8Eo_kShA), listeners=Endpoints(endpoints={ListenerName(CONTROLLER)=localhost/<unresolved>:9093}), supportedKRaftVersion=SupportedVersionRange[min_version:0, max_version:1])}) at offset -1 (org.apache.kafka.raft.internals.KRaftControlRecordStateMachine)
[2025-04-02 16:59:08,659] INFO [RaftManager id=1] Starting voters are VoterSet(voters={1=VoterNode(voterKey=ReplicaKey(id=1, directoryId=F7htgtxrSBykCD8Eo_kShA), listeners=Endpoints(endpoints={ListenerName(CONTROLLER)=localhost/<unresolved>:9093}), supportedKRaftVersion=SupportedVersionRange[min_version:0, max_version:1])}) (org.apache.kafka.raft.KafkaRaftClient)
[2025-04-02 16:59:08,666] INFO [RaftManager id=1] Attempting durable transition to UnattachedState(epoch=0, leaderId=OptionalInt.empty, votedKey=Optional.empty, voters=[1], electionTimeoutMs=1114, highWatermark=Optional.empty) from null (org.apache.kafka.raft.QuorumState)
[2025-04-02 16:59:08,691] INFO [RaftManager id=1] Completed transition to UnattachedState(epoch=0, leaderId=OptionalInt.empty, votedKey=Optional.empty, voters=[1], electionTimeoutMs=1114, highWatermark=Optional.empty) from null (org.apache.kafka.raft.QuorumState)
.
.
.
        ssl.keystore.certificate.chain = null
        ssl.keystore.key = null
        ssl.keystore.location = null
        ssl.keystore.password = null
        ssl.keystore.type = JKS
        ssl.principal.mapping.rules = DEFAULT
        ssl.protocol = TLSv1.3
        ssl.provider = null
        ssl.secure.random.implementation = null
        ssl.trustmanager.algorithm = PKIX
        ssl.truststore.certificates = null
        ssl.truststore.location = null
        ssl.truststore.password = null
        ssl.truststore.type = JKS
        telemetry.max.bytes = 1048576
        transaction.abort.timed.out.transaction.cleanup.interval.ms = 10000
        transaction.max.timeout.ms = 900000
        transaction.partition.verification.enable = true
        transaction.remove.expired.transaction.cleanup.interval.ms = 3600000
        transaction.state.log.load.buffer.size = 5242880
        transaction.state.log.min.isr = 1
        transaction.state.log.num.partitions = 50
        transaction.state.log.replication.factor = 1
        transaction.state.log.segment.bytes = 104857600
        transactional.id.expiration.ms = 604800000
        unclean.leader.election.enable = false
        unclean.leader.election.interval.ms = 300000
        unstable.api.versions.enable = false
        unstable.feature.versions.enable = false
 (org.apache.kafka.common.config.AbstractConfig)
[2025-04-02 16:59:09,588] INFO [BrokerServer id=1] Waiting for the broker to be unfenced (kafka.server.BrokerServer)
[2025-04-02 16:59:09,621] INFO [BrokerLifecycleManager id=1] The broker has been unfenced. Transitioning from RECOVERY to RUNNING. (kafka.server.BrokerLifecycleManager)
[2025-04-02 16:59:09,621] INFO [BrokerServer id=1] Finished waiting for the broker to be unfenced (kafka.server.BrokerServer)
[2025-04-02 16:59:09,622] INFO authorizerStart completed for endpoint PLAINTEXT. Endpoint is now READY. (org.apache.kafka.server.network.EndpointReadyFutures)
[2025-04-02 16:59:09,623] INFO [SocketServer listenerType=BROKER, nodeId=1] Enabling request processing. (kafka.network.SocketServer)
[2025-04-02 16:59:09,624] INFO Awaiting socket connections on 0.0.0.0:9092. (kafka.network.DataPlaneAcceptor)
[2025-04-02 16:59:09,626] INFO [BrokerServer id=1] Waiting for all of the authorizer futures to be completed (kafka.server.BrokerServer)
[2025-04-02 16:59:09,626] INFO [BrokerServer id=1] Finished waiting for all of the authorizer futures to be completed (kafka.server.BrokerServer)
[2025-04-02 16:59:09,626] INFO [BrokerServer id=1] Waiting for all of the SocketServer Acceptors to be started (kafka.server.BrokerServer)
[2025-04-02 16:59:09,626] INFO [BrokerServer id=1] Finished waiting for all of the SocketServer Acceptors to be started (kafka.server.BrokerServer)
[2025-04-02 16:59:09,626] INFO [BrokerServer id=1] Transition from STARTING to STARTED (kafka.server.BrokerServer)
[2025-04-02 16:59:09,628] INFO Kafka version: 4.0.0 (org.apache.kafka.common.utils.AppInfoParser)
[2025-04-02 16:59:09,628] INFO Kafka commitId: 985bc99521dd22bb (org.apache.kafka.common.utils.AppInfoParser)
[2025-04-02 16:59:09,628] INFO Kafka startTimeMs: 1743605949627 (org.apache.kafka.common.utils.AppInfoParser)
[2025-04-02 16:59:09,631] INFO [KafkaRaftServer nodeId=1] Kafka Server started (kafka.server.KafkaRaftServer)
```
Then after I enabled the WSL integration in the docker desktop, I got the Docker image:

```python
bmikes@bmikes:~/kafka_2.13-4.0.0$ sudo docker pull apache/kafka:4.0.0
[sudo] password for bmikes:
4.0.0: Pulling from apache/kafka
35e38a4b206f: Download complete
2fa1f65d07a3: Download complete
28bd55152645: Download complete
f18232174bc9: Download complete
729fc64ae8c1: Download complete
643bf8a7c247: Download complete
b8eeb529f1af: Download complete
a723193c2f26: Download complete
73851e29d6a7: Download complete
c3f73af09931: Download complete
b42f712acf6d: Download complete
Digest: sha256:3f7b939115cd4872e9cee9369d80bd69712fde55f9902f46d793f64848dedc75
Status: Downloaded newer image for apache/kafka:4.0.0
docker.io/apache/kafka:4.0.0
```
Started the Kafka Docker container: 
```python
bmikes@bmikes:~/kafka_2.13-4.0.0$ sudo docker run -p 9092:9092 apache/kafka:4.0.0
===> User
uid=1000(appuser) gid=1000(appuser) groups=1000(appuser)
===> Setting default values of environment variables if not already set.
CLUSTER_ID not set. Setting it to default value: "5L6g3nShT-eMCtK--X86sw"
===> Configuring ...
===> Launching ...
===> Using provided cluster id 5L6g3nShT-eMCtK--X86sw ...
[2025-04-02 15:18:19,302] INFO Registered kafka:type=kafka.Log4jController MBean (kafka.utils.Log4jControllerRegistration$)
[2025-04-02 15:18:19,746] INFO Registered signal handlers for TERM, INT, HUP (org.apache.kafka.common.utils.LoggingSignalHandler)
[2025-04-02 15:18:19,752] INFO [ControllerServer id=1] Starting controller (kafka.server.ControllerServer)
[2025-04-02 15:18:20,091] INFO Updated connection-accept-rate max connection creation rate to 2147483647 (kafka.network.ConnectionQuotas)
[2025-04-02 15:18:20,167] INFO [SocketServer listenerType=CONTROLLER, nodeId=1] Created data-plane acceptor and processors for endpoint : ListenerName(CONTROLLER) (kafka.network.SocketServer)
[2025-04-02 15:18:20,180] INFO CONTROLLER: resolved wildcard host to 4eead6fcca73 (org.apache.kafka.metadata.ListenerInfo)
[2025-04-02 15:18:20,194] INFO authorizerStart completed for endpoint CONTROLLER. Endpoint is now READY. (org.apache.kafka.server.network.EndpointReadyFutures)
[2025-04-02 15:18:20,197] INFO [SharedServer id=1] Starting SharedServer (kafka.server.SharedServer)
[2025-04-02 15:18:20,338] INFO [LogLoader partition=__cluster_metadata-0, dir=/tmp/kraft-combined-logs] Loading producer state till offset 0 (org.apache.kafka.storage.internals.log.UnifiedLog)
[2025-04-02 15:18:20,343] INFO [LogLoader partition=__cluster_metadata-0, dir=/tmp/kraft-combined-logs] Reloading from producer snapshot and rebuilding producer state from offset 0 (org.apache.kafka.storage.internals.log.UnifiedLog)
.
.
.
        transaction.state.log.num.partitions = 50
        transaction.state.log.replication.factor = 1
        transaction.state.log.segment.bytes = 104857600
        transactional.id.expiration.ms = 604800000
        unclean.leader.election.enable = false
        unclean.leader.election.interval.ms = 300000
        unstable.api.versions.enable = false
        unstable.feature.versions.enable = false
 (org.apache.kafka.common.config.AbstractConfig)
[2025-04-02 15:18:21,500] INFO [BrokerLifecycleManager id=1] The broker is in RECOVERY. (kafka.server.BrokerLifecycleManager)
[2025-04-02 15:18:21,502] INFO [BrokerServer id=1] Waiting for the broker to be unfenced (kafka.server.BrokerServer)
[2025-04-02 15:18:21,535] INFO [BrokerLifecycleManager id=1] The broker has been unfenced. Transitioning from RECOVERY to RUNNING. (kafka.server.BrokerLifecycleManager)
[2025-04-02 15:18:21,536] INFO [BrokerServer id=1] Finished waiting for the broker to be unfenced (kafka.server.BrokerServer)
[2025-04-02 15:18:21,538] INFO authorizerStart completed for endpoint PLAINTEXT. Endpoint is now READY. (org.apache.kafka.server.network.EndpointReadyFutures)
[2025-04-02 15:18:21,539] INFO [SocketServer listenerType=BROKER, nodeId=1] Enabling request processing. (kafka.network.SocketServer)
[2025-04-02 15:18:21,540] INFO Awaiting socket connections on 0.0.0.0:9092. (kafka.network.DataPlaneAcceptor)
[2025-04-02 15:18:21,543] INFO [BrokerServer id=1] Waiting for all of the authorizer futures to be completed (kafka.server.BrokerServer)
[2025-04-02 15:18:21,545] INFO [BrokerServer id=1] Finished waiting for all of the authorizer futures to be completed (kafka.server.BrokerServer)
[2025-04-02 15:18:21,547] INFO [BrokerServer id=1] Waiting for all of the SocketServer Acceptors to be started (kafka.server.BrokerServer)
[2025-04-02 15:18:21,547] INFO [BrokerServer id=1] Finished waiting for all of the SocketServer Acceptors to be started (kafka.server.BrokerServer)
[2025-04-02 15:18:21,548] INFO [BrokerServer id=1] Transition from STARTING to STARTED (kafka.server.BrokerServer)
[2025-04-02 15:18:21,550] INFO Kafka version: 4.0.0 (org.apache.kafka.common.utils.AppInfoParser)
[2025-04-02 15:18:21,550] INFO Kafka commitId: 985bc99521dd22bb (org.apache.kafka.common.utils.AppInfoParser)
[2025-04-02 15:18:21,550] INFO Kafka startTimeMs: 1743607101548 (org.apache.kafka.common.utils.AppInfoParser)
[2025-04-02 15:18:21,553] INFO [KafkaRaftServer nodeId=1] Kafka Server started (kafka.server.KafkaRaftServer)
```
## Step 3: Create a topic to store your events

 Kafka is a distributed event streaming platform that lets you read, write, store, and process events (also called records or messages in the documentation) across many machines.

Example events are payment transactions, geolocation updates from mobile phones, shipping orders, sensor measurements from IoT devices or medical equipment, and much more. These events are organized and stored in topics. Very simplified, a topic is similar to a folder in a filesystem, and the events are the files in that folder.

So before you can write your first events, you must create a topic. Open another terminal session and run: 
```python
bmikes@bmikes:~/kafka_2.13-4.0.0$ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
Created topic quickstart-events.
bmikes@bmikes:~/kafka_2.13-4.0.0$
```
All of Kafka's command line tools have additional options: run the kafka-topics.sh command without any arguments to display usage information. For example, it can also show you details such as the partition count of the new topic: 
```python
bmikes@bmikes:~/kafka_2.13-4.0.0$  bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
Topic: quickstart-events        TopicId: USw0vrwyTTSjfor2GZhFaw PartitionCount: 1       ReplicationFactor: 1    Configs: segment.bytes=1073741824
        Topic: quickstart-events        Partition: 0    Leader: 1       Replicas: 1     Isr: 1  Elr:    LastKnownElr:
bmikes@bmikes:~/kafka_2.13-4.0.0$
```
## Step 4: Write some events into the topic

 A Kafka client communicates with the Kafka brokers via the network for writing (or reading) events. Once received, the brokers will store the events in a durable and fault-tolerant manner for as long as you need—even forever.

Run the console producer client to write a few events into your topic. By default, each line you enter will result in a separate event being written to the topic. 
```python
bmikes@bmikes:~/kafka_2.13-4.0.0$  bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
>This is my first event
>This is my second event
>This is my last event
>^CCommand 'is' not found, but can be installed with:
sudo apt install ironseed
Command 'is' not found, but can be installed with:
sudo apt install ironseed
```

## Step 5: Read the events

Open another terminal session and run the console consumer client to read the events you just created:
```python
bmikes@bmikes:~/kafka_2.13-4.0.0$  bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
This is my last event

^CProcessed a total of 1 messages
bmikes@bmikes:~/kafka_2.13-4.0.0$
```
