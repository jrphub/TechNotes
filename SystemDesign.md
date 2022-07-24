# System Design Notes



## Reference

Youtube : Coding Simplified : https://www.youtube.com/watch?v=vge7qwCR1dA&list=PLt4nG7RVVk1g_LutiJ8_LvE914rIE5z4u



### Load Balancer

- Hardware LB
- Software LB 
  - HAProxy is popular open source LB
  - Agorithm used
    - Round Robin
    - Round Robin with weighted server
    - Least connections
    - Least response time
    - source IP hash
    - URL hash

### Cache

#### Types

- Application Server Cache : Placing cache on request layer node enables local storage
  - For multiple node with LB, cache miss can happen
  - Solution : Distributed Cache and Content Distributed Network (CDN)
- Distributed Cache : 
  - Each node has its own cache
  - using consisted hashing function : request is redirected to a particular node
- Global Cache :
  - All nodes use the same single cache space
- CDN : For serving large amount of static media which is common to all

#### Cache invalidation

- When data is modified in DB, it should be invalidate the cache
- Types
  - Write-through Cache:
    - DB contains all data, cache contains only important data. While persisting data, store data in both cache and DB
    - Disadvantage : higher latency for write
  - Write Around Cache
    - Data is written directly to storage, bypassing the cache
    - Disadvantage : a read request for recently written data will create a cache miss and must be read from slower backend
  - Write Back Cache
    - Update the data in cache first. In regular interval, persist the data into DB
    - Disadvantage : Risk of data loss in case of disk crash

#### Cache Eviction Policie

- FIFO - not suitable
- LIFO - not suitable
- **LRU - Useful**
- Most recently used
- Least Frequently Used
- Random Replacement

### Sharding - Data Partitioning

- Sharding is dividing the data into multiple chunks
- Replication factor 
- Horizontal scaling - adding more machines, which is cheaper and feasible - preferable
- Vertical scaling - upgrade the capacity of a machine - not suitable
- Sharding Methods :
  - Horizontal partitioning : Range based sharding : ex- Based on Alphabetical names
  - Vertical Partitioning : Divide tables based of feature i.e. 1 feature in 1 DB
  - Directory based partitioning : Each row has its own DB number
- Sharding Criteria :
  - Hash based : Using hashcode on any entity value - Not Balanced
  - List Partitioning : Based on list i.e Regions: APAC, EMEA, US - Not balanced
  - Round robin partitioning : Can be balanced
  - Composite Partitiong : Combine any above scheme
- Sharding Challenges
  - ACID compliance
  - Joins inefficient

### Indexes

- Useful in database to improve the speed of data retrieval
- pointer to actual data
- it is a data structure of table of contents that points us to the location where actual data lives
- Types of Indexing
  - Ordered Indexing - Column is sorted as per ascending order
  - Hash Indexing - Indexing as per Hash function and Hash Table

### Proxy Server

- It sits between client and server
- Filter the request- ACL controlled
- Transform the header of the request
- Optimize the request traffic
- collapse request for spatially close together
- Provides anonymity

### Message Queues

- Async service-to-service communication used in serverless and microservices architectures
- ActiveMQ, Kafka

### SQL vs No-SQL

- SQL - Vertically scalable - static schema, ACID complianced, structured data
- NoSQL - dynamic schema, horizontal scalable, not much ACID complianced, designed to be scaled across the region, large volume of data
  - Key-Value : Redis, Dynamo
  - Document based - MongoDB, CouchDB
  - Wide column database - Column familes, Hbase, Cassendra
  - Graph Database - Neo4j, infine graph

### Monolithic vs. Microservices

- Monolithic - all software components are assembled together and tightly coupled
- Microservices - Different services for different component and interact with each other through RESTful
  - one for search, other for notification, index etc.
  - Each have their LB, cache, indexes, REST api
  - Separate datastore
  - separate as per features
  - server which are stateless