#  E-Commerce Database Schema


## 1️⃣ Customers

```sql
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(20),
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

---

## 2️⃣ Addresses

(One customer can have multiple addresses)

```sql
CREATE TABLE Addresses (
    address_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    address_line VARCHAR(255) NOT NULL,
    city VARCHAR(100) NOT NULL,
    state VARCHAR(100),
    country VARCHAR(100) NOT NULL,
    postal_code VARCHAR(20) NOT NULL,
    address_type ENUM('Billing', 'Shipping') NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
        ON DELETE CASCADE
);
```

---

## 3️⃣ Categories

```sql
CREATE TABLE Categories (
    category_id INT PRIMARY KEY AUTO_INCREMENT,
    category_name VARCHAR(100) NOT NULL UNIQUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

---

## 4️⃣ Products

```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(150) NOT NULL,
    description TEXT,
    price DECIMAL(10,2) NOT NULL CHECK (price >= 0),
    stock INT NOT NULL DEFAULT 0 CHECK (stock >= 0),
    category_id INT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES Categories(category_id)
        ON DELETE RESTRICT
);
```

---

## 5️⃣ Orders

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status ENUM('Pending', 'Processing', 'Shipped', 'Delivered', 'Cancelled') 
           NOT NULL DEFAULT 'Pending',
    shipping_address_id INT NOT NULL,
    billing_address_id INT NOT NULL,
    total_amount DECIMAL(12,2) DEFAULT 0,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
        ON DELETE CASCADE,
    FOREIGN KEY (shipping_address_id) REFERENCES Addresses(address_id),
    FOREIGN KEY (billing_address_id) REFERENCES Addresses(address_id)
);
```

---

## 6️⃣ Order Items

(Each order can contain multiple products)

```sql
CREATE TABLE Order_Items (
    order_item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    price DECIMAL(10,2) NOT NULL CHECK (price >= 0),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
        ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
        ON DELETE RESTRICT,
    UNIQUE (order_id, product_id)
);
```

---

## 7️⃣ Payments

```sql
CREATE TABLE Payments (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    payment_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    amount DECIMAL(12,2) NOT NULL CHECK (amount >= 0),
    payment_method ENUM('Credit Card', 'Debit Card', 'PayPal', 'Bank Transfer', 'Cash') NOT NULL,
    payment_status ENUM('Pending', 'Completed', 'Failed', 'Refunded') 
                   NOT NULL DEFAULT 'Pending',
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
        ON DELETE CASCADE
);
```

---

#  Indexes (For Performance Practice)

```sql
CREATE INDEX idx_orders_customer ON Orders(customer_id);
CREATE INDEX idx_products_category ON Products(category_id);
CREATE INDEX idx_order_items_order ON Order_Items(order_id);
CREATE INDEX idx_payments_order ON Payments(order_id);
```

---

#  Final Relationships Overview

* Customers (1) → (∞) Addresses
* Customers (1) → (∞) Orders
* Categories (1) → (∞) Products
* Orders (1) → (∞) Order_Items
* Products (1) → (∞) Order_Items
* Orders (1) → (∞) Payments

---
