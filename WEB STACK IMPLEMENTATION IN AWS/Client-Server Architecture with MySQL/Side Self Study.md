# DevOpsTraining
**DevOps/Cloud Training Material**

# Side Self Study

## 1. Read about Ping and Traceroute Network Diagnostic Utilities

### Ping

Ping is a network utility used to test the reachability of a host on an IP network and to measure the round-trip time for messages sent from the originating host to a destination computer. It uses Internet Control Message Protocol (ICMP) Echo Request messages and expects Echo Reply messages in response.

**Usage**:
```sh
ping <destination>
```

**Key Results**:
- **Packets Sent/Received/Lost**: Indicates the number of packets sent and the number of responses received.
- **Round-Trip Time (RTT)**: Shows the time it takes for a packet to travel to the destination and back.
  - **min**: Minimum RTT observed.
  - **avg**: Average RTT observed.
  - **max**: Maximum RTT observed.
  - **mdev**: Mean deviation of RTT.

### Traceroute

Traceroute is a network diagnostic tool used to track the path packets take from one IP address to another. It helps in identifying routing paths and potential network bottlenecks or failures.

**Usage**:
```sh
traceroute <destination>
```

**Key Results**:
- **Hops**: Each line represents a hop, which is a step from one router to another.
- **RTT for Each Hop**: Shows the time it took for the packet to travel to that hop and back.
- **Hop IP Addresses/Hostnames**: Provides the IP addresses or hostnames of each hop.

By understanding these tools, you can diagnose network issues, measure latency, and identify points of failure in a network path.

## 2. Refresh Your Knowledge of Basic SQL Commands

### SHOW

The `SHOW` command is used to display information about databases, tables, and other database objects.

**Examples**:
```sql
SHOW DATABASES;
SHOW TABLES;
SHOW COLUMNS FROM <table_name>;
```

### CREATE

The `CREATE` command is used to create databases, tables, and other objects.

**Examples**:
```sql
CREATE DATABASE mydatabase;
CREATE TABLE mytable (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    age INT,
    email VARCHAR(255) UNIQUE
);
```

### DROP

The `DROP` command is used to delete databases, tables, and other objects.

**Examples**:
```sql
DROP DATABASE mydatabase;
DROP TABLE mytable;
```

### SELECT

The `SELECT` command is used to query data from a database. It can be used to retrieve specific columns, filter rows, and perform aggregations.

**Examples**:
```sql
SELECT * FROM mytable;
SELECT name, email FROM mytable WHERE age > 30;
SELECT COUNT(*) FROM mytable;
```

### INSERT

The `INSERT` command is used to add new rows to a table.

**Examples**:
```sql
INSERT INTO mytable (name, age, email) VALUES ('John Doe', 25, 'john@example.com');
INSERT INTO mytable (name, age, email) VALUES ('Jane Smith', 30, 'jane@example.com');
```
