## Deep Dives

For the main components, we looked at the problems we can face as the system grows, the different options, and why we decided to use a specific one.

First, the client will send a request to the system, and the system will perform authentication and authorization using JWT to check whether the client is a customer or operator
Then the client will connect to the load balancer, which will redirect it to the server, and the servers are completely stateless, so there will be a cash sessions based workflow to make the servers reliable and consistent

For the order-making workflow, when an cusmoer finshing for his money transaction for his order, his order will enter a message queue for allocating the robots for his order to fulfill it, and that is making the confirmation of the orders to the customers within our range of 500ms and even less

Since we are targeting 100 M orders per month, this means around 70 orders per second, so we will need 70 workers for the message quue wihci making our system pretty fast and reliable and makes the time of allocating the robots to the items is less than our maximum target of 2 sec, and the system will make sure that there is no similarity between any item_id and robot_id if the status was stated as completed 

For any failure in the allocation of items to robots, the system will push an event to the notifications message queue and will reallocate a robot for the item

The system will send checks for the robots every 5 sec to see the status of the item they are allocated for to see if it is done or not, and manage the individual orders based on that to see their status

As the frequency of reads to writes is high, there will be replicas for read operations on the database, and if there is a write operation, it could be on the main database. This will create a small delay in the replica, which could affect the user experience a little bit, but it is acceptable in our system

For the notifications, it will be handled with a message queue, as it is not required for the user to wait for it as it will be sent automatically upon tracking of any event
