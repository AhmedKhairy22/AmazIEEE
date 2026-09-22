## Deep Dives

For the main components, we looked at the problems we can face as the system grows, the different options, and why we decided to use a specific one.

First, the client will send a request to the system, and the system will perform authentication and authorization using JWT to check whether the client is allowed to see certain content or not
Then the client will connect to the load balancer, which will redirect it to the server, and the servers are completely stateless, so there will be a cash sessions based workflow to make the servers reliable and consistent

Since the database has a limited number of connection i will make a connection pool; those are always connected to the database, and if the server needs to make any operation on the database, it could ask the pool. And it will be PgBouncer, as we are using Postgres

As the frequency of reads to writes is high, there will be replicas for read operations on the database, and if there is a write operation, it could be on the main database. This will create a small delay in the replica, which could affect the user experience a little bit, but it is acceptable in our system

For the notifications, it will be handled with a message queue, as it is not required for the user to wait for it

For any order journey in the system, when a customer confirms an order, the system takes the details of the order and the number of the items, and so on, and then assigns the number of robots to the items, making sure that the order is not processed twice by checking on the item and robot id  
