# Data_Modelling_Design_CDE
### Overview:
Fufu Republic is a popular restaurant chain in Nigeria with multiple outlets nationwide. While the
core menu is standardized, some items vary by location (e.g., the Agege branch may sell
Chinese Rice, while the Lekki branch might not). Customers can order online through the
website or visit outlets for dine-in or take-out. The restaurant aims to levarage data to:

● Understand sales trends across locations, payment methods, and dining options
(dine-in, take-out, online).
● Manage stock levels efficiently, reducing waste and ensuring availability.
● Enhance customer experience by analyzing purchasing habits and tailoring promotions
accordingly.

The dimensional model below have been designed to achieve these objectives.

1. Entities, Relationships and Constraints
Entities:
1. # Branch
Each outlet (branch) of the restaurant.
Attributes:
branch_id: Unique identifier (Primary Key)
branch_name: Name of the branch (e.g., Agege, Lekki)
branch_location: Physical location

2. # Menu Item
Represents the different items on the menu.
**Attributes**:
**item_id**: Unique identifier (Primary Key)
**item_name**: Name of the menu item (e.g., Chinese Rice)
**item_category**: Category (e.g., Main Dish, Beverage)

3. # Customer
Represents a person who dines in or orders online.
Attributes:
**customer_id**: Unique identifier (Primary Key)
**firstname**: First name of the customer
**lastname**: Last name of the customer
**gender**: Customer's gender
**Phone**: Customer's phone number

4. # Sales Transaction
Represents each individual sale made.
**Attributes**:
**sales_transaction_id**: Unique identifier (Primary Key)
**date_id**: Date of the transaction
**branch_id**: Reference to the branch where the transaction occurred
**item_id**: Reference to the purchased item
**customer_id**: Reference to the customer (if known)
**payment_id**: Reference to the payment method used
**quantity_sold**: Number of units sold
**total_amount**: Total sales amount

5. # Payment Method
Represents different ways customers can pay.
**Attributes**:
**payment_id**: Unique identifier (Primary Key)
**payment_type**: Type of payment (e.g., Cash, POS, Online)
**payment_gateway**: Payment processor used (for online payments)

6. # Date
Represents the date dimension for reporting sales trends.
Attributes:
**date_id**: Unique identifier (Primary Key)
**date**: Actual date
**day_of_week**: Day of the week
**month**: Month
**quarter**: Quarter
**year**: Year

7. # Stock Inventory
Tracks inventory levels at each branch for menu items.
Attributes:
**branch_id**: Reference to the branch
**item_id**: Reference to the menu item
**stock_level**: Current stock level

8. # Order Dimension
**order_id**: Unique order ID
**order_type**: Type (dine-in, take-out, online)
**price**: Amount for each order placed.

## Relationships:
Branch ↔ Menu Item:

A branch can serve multiple menu items, and each menu item can be available at multiple branches.
Relationship: Many-to-Many (Resolved by Sales Fact Table)
Customer ↔ Sales Transaction:

A customer can make multiple sales transactions, but each sales transaction is tied to one customer.
Relationship: One-to-Many
Sales Transaction ↔ Branch:

Each sales transaction occurs at a specific branch.
Relationship: Many-to-One
Sales Transaction ↔ Payment Method:

Each sales transaction uses one payment method.
Relationship: Many-to-One
Sales Transaction ↔ Menu Item:

Each sales transaction involves one or more menu items.
Relationship: Many-to-One (through a fact table)

Stock Inventory ↔ Branch:

Each branch maintains inventory for the items it sells.
Relationship: One-to-One
Stock Inventory ↔ Menu Item:

Each menu item can be stocked in multiple branches.
Relationship: One-to-One
Constraints:
Primary Keys:

Ensure each entity has a unique identifier (branch_id, item_id, customer_id, etc.).
Foreign Keys:

Sales Transaction references branch_id, item_id, customer_id, and payment_id to link each sale to the branch, customer, and payment method.
Stock Inventory references branch_id and item_id to link the inventory to a specific branch and menu item.
Unique Constraints:

Each combination of branch_id and item_id in the Stock Inventory table should be unique.
Not Null Constraints:

Attributes like sales_transaction_id, branch_id, and item_id should not allow NULL values to ensure data integrity.

2. ## Dimensional Model
Business Process: 
A good business process to focus on would be sales transactions, which allows us to answer business questions about customer behavior, payment methods, and stock levels.

## Business Questions:
What are the sales trends across different branches?
Which menu items sell the most?
How do customers prefer to pay (cash, POS, or online payments)?
Which items are frequently bought together?
How do customer preferences vary by location?

# Grain:
The grain (or level of detail) is each individual sales transaction.

# Dimensions:
# Branch Dimension:

Branch ID (Primary Key)
Branch Location (e.g., Agege, Lekki)

# Menu Item Dimension:

Item ID (Primary Key)
Item Name (e.g., Chinese Rice, Jollof Rice)
Category (e.g., Main Dish, Beverage)

# Customer Dimension:

Customer ID (Primary Key)
**firstname**: First name of the customer
**lastname**: Last name of the customer
**gender**: Customer's gender
**Phone**: Customer's phone number

# Payment Method Dimension:
Payment ID (Primary Key)
Payment Type (Cash, POS, Online)
Payment Gateway (if online)

# Date Dimension:
Date ID (Primary Key)
Date (actual date)
Week, Month, Quarter, Year

## Fact Table: Sales Fact
# Grain: 
Each row in this table represents one sales transaction at the lowest level (per customer, per item, per transaction).
# Columns:
Date ID (Foreign Key)
Branch ID (Foreign Key)
Item ID (Foreign Key)
Customer ID (Foreign Key)
Payment ID (Foreign Key)
Quantity Sold
Total Amount
