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

## Data Model 
I chose PostgreSQL as the primary source of truth because the system contains highly related and transactional data, including:
- Users ( have the user_id, name, age, location)
- Products ( product_id, thumbnail_link, desc, price, vendor, category)
- Orders ( user_id, product_id (just a note here: the product ID could be separated by a comma since the SQL database schema is restricted), date, money)
- robots (robot_id, shelf_life, battry capactiy)
- fulfillments (id, robot_id, item_id, date_started, date_fulfilled, status). A validation will be on the item_id and robot_id to make sure that the same item is not picked twice 
- items (item_id, product_id)
- notifications ( id, event, status)

## API Design 
I chose a RESTful HTTP architecture  because the domain revolves around well-defined, persistent resources and a data model. REST allows us to scale stateless API servers horizontally behind a load balancer and leverage standard HTTP caching for high-frequency queries like product listings.

| Endpoint | Request_body | Response | Purpose |
|---|---|---|---|
| `POST /users/{userId}/profile` | Profile data: name, locaion, cerdit_informatinns | Profile | Create/update custmoer profile |
| `POST /products` | product details, description, price. | product_id | Create a product |
| `PATCH /products/{product_id}` | Fields to update | Updated product | Update a product |
| `GET /products?keywords={keywords}&page={page}&limit={limit}` | Filters: keywords, price, category, pagination | Paginated products | Producs search |
| `GET /users/{userId}/profile` | the user_id | profile data (previous orders, name, location, and so on | get a user profile|
| `GET /orders/{userId}` | the user_id | orders_ids and info | get the order for a specific user |
| `GET /orders` | Filters: keywords, money, date, pagination | orders_ids and info| get all the orders|
| `POST /robots` | robot info | robot ID + status | add a robot to the fleet|
| `PATCH /robots` | robot info the needed to be updatted | robot ID + status | update the robot to the fleet|
| `DELETE /robots/{robot_id}` | robot_id | robot ID + status | delete a robot form the fleet|
| `POST /robots/{robot_id}` | Candidate ID | robot ID + status | Apply for a job |

