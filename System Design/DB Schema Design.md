# Database Schema Design – Online Food Delivery App

This document contains a **DB schema design interview question** and a **production-ready solution** suitable for **2–3 YOE backend / full-stack roles**.

---

## 📌 Problem Statement

Design a **relational database schema** for an **online food delivery platform** (e.g., Swiggy / Zomato).

---

## 🔹 Functional Requirements

1. **Users**

   - Sign up and log in using mobile number
   - Can have multiple delivery addresses

2. **Restaurants**

   - Can list multiple food items
   - Can be open or closed

3. **Menu**

   - Each restaurant has multiple food items
   - Each item has a current price and availability

4. **Orders**

   - A user can place multiple orders
   - Each order belongs to exactly one restaurant
   - An order can have multiple food items with quantity

5. **Payments**

   - Each order has exactly one payment
   - Payment methods: `UPI`, `CARD`, `CASH`
   - Payment status: `SUCCESS`, `FAILED`, `PENDING`

6. **Order Tracking**

   - Order statuses:
     - `PLACED`
     - `PREPARING`
     - `OUT_FOR_DELIVERY`
     - `DELIVERED`
     - `CANCELLED`

7. **Reviews**
   - Users can rate and review a restaurant
   - Review is allowed only after order is delivered
   - One review per order

---

## 🔹 Constraints

- Food item price must be stored **at order time**
- Historical orders must not be affected by menu changes
- Avoid data redundancy
- Use proper primary keys and foreign keys

---

## Database Schema Design

---

### 1️⃣ Users

- id (PK)
- mobile_no (UNIQUE, NOT NULL)
- name
- email (UNIQUE, NULLABLE)
- created_at

### 2️⃣ User_Addresses

- id (PK)
- user_id (FK → Users.id)
- address_line
- city
- pincode
- is_default
- created_at

### 3️⃣ Restaurants

- id (PK)
- restaurant_name
- is_open
- created_at

### 4️⃣ Menu_Items

- id (PK)
- restaurant_id (FK → Restaurants.id)
- item_name
- current_price
- is_available

### 5️⃣ Orders

- id (PK)
- user_id (FK → Users.id)
- restaurant_id (FK → Restaurants.id)
- user_address_id (FK → User_Addresses.id)
- status
- total_amount
- created_at

### 6️⃣ Order_Items

- id (PK)
- order_id (FK → Orders.id)
- menu_item_id (FK → Menu_Items.id)
- quantity
- price_at_order_time

### 7️⃣ Payments

- id (PK)
- order_id (FK → Orders.id, UNIQUE)
- payment_method (UPI, CARD, CASH)
- status (SUCCESS, FAILED, PENDING)
- amount
- paid_at

### 8️⃣ Reviews

- id (PK)
- order_id (FK → Orders.id, UNIQUE)
- restaurant_id (FK → Restaurants.id)
- user_id (FK → Users.id)
- rating (1–5)
- comments
- created_at
