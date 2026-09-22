## Functional Requirements 
- Customers can create accounts
- Customers can browse products and order them, and they should be able to track the status of what they ordered in the real time, with notifications sent upon changes to this status
- Upon order being received, the system should validate the existence of the items and the counts in the inventory
- The system should make sure that the order is never processed twice
- The system will take the order and will generate the number of robots with the assigned items for them
- The system will receive a status for each robot at a certain time interval to track the status of the orders, and the system should handle the failure of any robot for any given reason
- The system will show a complete overview status for any order and any robots at any given time
- There is a notification alert for some of the important events, like low stock or a failure of any robot or fulfillment of any order, and you could specify the exact event you need to push notifications for

## Non-Functional Requirements 
- For scalability, the system could scale horizontally and should handle over 100M orders per month and high read-to-write ratios (100:1 for search vs. ordering).
- The system should maintain a high level of availability and consistency
- The order confirmation for the customers should not take more than 500ms
- The time from receiving the order to the system and allocating the robots should not take more than 10 secs 
