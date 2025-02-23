## Database Structure
### Users

id | name | email | password | address | role
---|------|-------|----------|---------|-----
Primary Key, INT | VARCHAR | VARCHAR | VARCHAR | VARCHAR | ENUM (ADMIN/CUSTOMER)

---

### Products
  
id | product_name | description | price | stock | category_id | img_url
---|--------------|-------------|-------|-------|------------|--------
Primary Key, INT | VARCHAR | VARCHAR | INT | INT | Foreign Key -> categories | VARCHAR

---

### Categories

id | category
---|---------
Primary Key, INT | ENUM(STAPLE_FOODS, INSTANT_PROCESSED, BEVERAGES_COFFEE, SPICES_HERBS, FRESH_FOODS, HOUSEHOLD_CLEANING)

---

### Orders
id | user_id | total_price | status
---|---------|-------------|-------
Primary Key, INT | Foreign Key -> Users | INT | ENUM(PENDING, PROCESS, SUCCESS)

---

### Carts
id | order_id | product_id |
---|---------|------------|
Primary Key, INT | Foreign Key -> Orders | Foreign Key -> Products |

---

### cart_product (pivot)
id | cart_id | product_id | quantity | price
---|---------|------------|----------|-------
Primary Key, INT | Foreign Key -> Carts | Foreign Key -> Products | INT | INT

---

### Payments
id | order_id | payment_method | status
---|----------|----------------|-------
Primary Key, INT | Foreign Key -> Orders | ENUM(TF, E-WALLET, COD) | ENUM(PENDING, SUCCESS)

---

### Receipts
id | order_id | receipt_url
---|----------|------------
Primary Key, INT | Foreign Key -> Orders | VARCHAR

***

## Features

### 1. Register & Login
* User can register and login
* Available 2 roles (CUSTOMER, ADMIN)
* ADMIN: 
  * update & create product list (Create, Read, Update, Delete)
  * update order status (pending/process/success)
* CUSTOMER: 
  * Can order product

### Frontend
* Register :
```json
{
    "name": string,
    "email": string,
    "password": string,
    "password_confirmation": string
}
```
* Login :
```json
{
    "email": string,
    "password": string
}
```
* Logout :
```json
{
    "token": string
}
```

### Backend
* Register :
  * receive credentials (name, email, password, password_confirmation)
  * if valid create new user
* Login :
  * receive credentials (email, password)
  * if valid create token
  * passing token to Frontend
* Logout :
  * receiving token
  * if token validated then logout
---
### 2. Prouct Management
* Admin :
  * can doing CRUD for product table
  * can update order status (pending/process/success)
* Customer :
  * can order product

### Frontend
* Create Product :
```json
{
    "product_name": string,
    "description": string,
    "price": int,
    "stock": int,
    "category_id": int,
    "img_url": string
}
```
* Update Product :
```json
{
    "product_name": string,
    "description": string,
    "price": int,
    "stock": int,
    "category_id": int,
    "img_url": string
}
```
* Delete Product :
```json
{
    "id": int
}
```
* Update Order Status :
```json
{
    "order_id": int,
    "status": string
}
```

### Backend
* Create Product :
  * receive credentials (product_name, description, price, stock, category_id, img_url)
  * if valid create new product
* Update Product :
  * receive credentials (product_name, description, price, stock, category_id, img_url)
  * if valid update product
* Delete Product :
  * receive id
  * if valid delete product
* Update Order Status :
  * receive order_id, status
  * if valid update order status

---
### 3. Product Ordering
* Customer:
  * can add products to available order
  * can add products to new order
  * can delete products on cart
  * can checkout cart to order

### Frontend
* Add to available Order :
```json
{
    "product_id": int,
    "quantity": int,
    "order_id": int
}
```
* Add to new Order :
```json
{
    "product_id": int,
    "quantity": int
}
```
* Delete from Cart :
```json
{
    "product_id": int
}
```
* Checkout :
```json
{
    "payment_method": string
}
```

### Backend
* Add to Cart :
  * receive credentials (product_id, quantity)
  * if valid add product to cart
* Delete from Cart :
  * receive credentials (product_id)
  * if valid delete product from cart
* Checkout :
  * on payment feature for more information
---
### 4. Payment
* Customer:
  * can choose payment method
  * can complete payment for an order

### Frontend
* Choose payment method:
```json
{
    "order_id": int,
    "payment_method": string,
}
```
* Complete payment:
```json
{
    "order_id": int
}
```

### Backend
* Choose payment method:
  * receive credentials (order_id, payment_method)
  * if valid create new payment with status pending
* Complete payment:
  * receive credentials (order_id)
  * if valid update payment status to success

---
### 5. Digital **Receipt**