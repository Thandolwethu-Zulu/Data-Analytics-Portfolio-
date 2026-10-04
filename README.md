# 📊 Data Analytics Portfolio

![SQL](https://img.shields.io/badge/SQL-Basic-blue)
![Excel](https://img.shields.io/badge/Excel-Intermediate-green)
![Power BI](https://img.shields.io/badge/Power%20BI-Learning-yellow)
![Data Analytics](https://img.shields.io/badge/Data%20Analytics-Portfolio-orange)


---
# Database ERD Project

## Overview
This project demonstrates my understanding of relational databases
and Entity Relationship Diagrams (ERDs).

## Skills Demonstrated
- Database design
- Entity Relationship Diagrams
- Primary keys
- Foreign keys
- Relationships
- Cardinality
- Relational databases
- SQL

## Project Description
I designed an ERD showing the relationships between entities in a
relational database.

## Tools Used
- Draw.io
- SQL
- GitHub
# WitleShop Online Retail System - Database Design & ERD

## 1. Identified Entities and Attributes
 
### Customer
- **Customer_ID** (PK)
- Full_Name
- Email (Unique)
- Phone_Number
- Registration Date

### Customer_Address (Needed, because a customer can have multiple delivery addresse's ) 
- **Address_ID** (PK)
- *Customer_ID* (FK)
- Street_Address
- City
- Postal_Code
- Province

### Category
- **Category_ID** (PK)
- Category_Name
- Description

### Supplier
- **Supplier_ID** (PK)
- Supplier_Name
- Contact_Email
- Phone_Number

### Product
- **Product_ID** (PK)
- *Category_ID* (FK)
- *Supplier_ID* (FK)
- Product_Name
- Description
- Price
- Stock_Quantity

### Orders
- **Order_ID** (PK)
- *Customer_ID* (FK)
- Order_Date
- Order_Status (Pending, Shipped, Delivered, Cancelled)
- Total_Amount

### Order_Item 
- **Order_Item_ID** (PK)
- *Order_ID* (FK)
- *Product_ID* (FK)
- Quantity
- Unit_Price

### Payment
- **Payment_ID** (PK)
- *Order_ID* (FK, Unique)
- Payment_Date
- Payment_Method (Card, EFT, PayFast)
- Payment_Status
- Amount_Paid

### Delivery
- **Delivery_ID** (PK)
- *Order_ID* (FK, Unique)
- *Address_ID* (FK) (links to the Customers delivry address)
- Delivery_Date
- Delivery_Status
- Courier_Name
- Tracking_Number

---

## 2. Primary Keys (PK) & Foreign Keys (FK) Summary

|  Entity | Primary Key (PK) | Foreign Key(s) (FK) | References Entity |
| :--- | :--- | :--- | :--- |
| **Customer** | `Customer_ID` | *None* | — |
| **Customer_Address** | `Address_ID` | `Customer_ID` | `Customer(Customer_ID)` |
| **Category** | `Category_ID` | *None* | — |
| **Supplier** | `Supplier_ID` | *None* | — |
| **Product** | `Product_ID` | `Category_ID`<br>`Supplier_ID` | `Category(Category_ID)`<br>`Supplier(Supplier_ID)` |
| **Orders** | `Order_ID` | `Customer_ID` | `Customer(Customer_ID)` |
| **Order_Item** | `Order_Item_ID` | `Order_ID`<br>`Product_ID` | `Orders(Order_ID)`<br>`Product(Product_ID)` |
| **Payment** | `Payment_ID` | `Order_ID` | `Orders(Order_ID)` |
| **Delivery** | `Delivery_ID` | `Order_ID`<br>`Address_ID` | `Orders(Order_ID)`<br>`Customer_Address(Address_ID)` |

---

## 3. Relationships and Cardinality

1. **Customer to Customer_Address (1:M)**
   - A customer can have one or many registered delivery addresses.
   - Each delivery address belongs to exactly one customer.

2. **Customer to Orders (1:M)**
   - A customer can place zero, one, or many orders.
   - Each order belongs to exactly one customer.

3. **Category to Product (1:M)**
   - A category contains one or many products.
   - Each product belongs to exactly one category.

4. **Supplier to Product (1:M)**
   - A supplier provides one or many products.
   - Each product is supplied by exactly one supplier.

5. **Orders to Product (M:N resolved via `Order_Item`)**
   - An order can contain multiple products, and a product can appear in multiple orders.
   - Resolved into two 1:M relationships:
     - `Orders` to `Order_Item` (1:M)
     - `Product` to `Order_Item` (1:M)

6. **Orders to Payment (1:1)**
   - Each order has exactly one payment record.
   - Each payment belongs to exactly one order.


7. **Orders to Delivery (1:1)**
   - Each order has exactly one delivery record.
   - Each delivery record corresponds to exactly one order.

8. **Customer_Address to Delivery (1:M)**
   - A registered customer address can be used for one or many deliveries.
   - Each delivery must be linked to exactly one customer delivery address.

---

## 4. Entity-Relationship Diagram (ERD)

```mermaid
erDiagram
    CUSTOMER ||--o{ CUSTOMER_ADDRESS : "has"
    CUSTOMER ||--o{ ORDERS : "places"
    CATEGORY ||--|{ PRODUCT : "classifies"
    SUPPLIER ||--|{ PRODUCT : "supplies"
    ORDERS ||--|{ ORDER_ITEM : "contains"
    PRODUCT ||--o{ ORDER_ITEM : "included_in"
    ORDERS ||--|| PAYMENT : "paid_with"
    ORDERS ||--|| DELIVERY : "fulfilled_by"
    CUSTOMER_ADDRESS ||--o{ DELIVERY : "ships_to"

    CUSTOMER {
        int Customer_ID PK
        string Full_Name
        string Email
        string Phone_Number
        date Date_of_Registration
    }

    CUSTOMER_ADDRESS {
        int Address_ID PK
        int Customer_ID FK
        string Street_Address
        string City
        string Postal_Code
        string Province
    }

    CATEGORY {
        int Category_ID PK
        string Category_Name
        string Description
    }

    SUPPLIER {
        int Supplier_ID PK
        string Supplier_Name
        string Contact_Email
        string Phone_Number
    }

    PRODUCT {
        int Product_ID PK
        int Category_ID FK
        int Supplier_ID FK
        string Product_Name
        string Description
        decimal Price
        int Stock_Quantity
    }

    ORDERS {
        int Order_ID PK
        int Customer_ID FK
        date Order_Date
        string Order_Status
        decimal Total_Amount
    }

    ORDER_ITEM {
        int Order_Item_ID PK
        int Order_ID FK
        int Product_ID FK
        int Quantity
        decimal Unit_Price
    }

    PAYMENT {
        int Payment_ID PK
        int Order_ID FK
        date Payment_Date
        string Payment_Method
        string Payment_Status
        decimal Amount_Paid
    }

    DELIVERY {
        int Delivery_ID PK
        int Order_ID FK
        int Address_ID FK
        date Delivery_Date
        string Delivery_Status
        string Courier_Name
        string Tracking_Number
    }
