# E-Commerce Database Sample Data

## Insert Customers

INSERT INTO Customers VALUES
(1, 'John', 'Doe', 'john@example.com', '1234567890', '2024-01-10'),
(2, 'Alice', 'Smith', 'alice@example.com', '9876543210', '2024-02-15'),
(3, 'Bob', 'Johnson', 'bob@example.com', '5556667777', '2024-03-01'),
(4, 'Emma', 'Brown', 'emma@example.com', '1112223333', '2024-03-20');

---

## Insert Categories

INSERT INTO Categories VALUES
(1, 'Electronics'),
(2, 'Clothing'),
(3, 'Books');

---

## Insert Products

INSERT INTO Products VALUES
(1, 'Laptop', 800.00, 10, 1),
(2, 'Smartphone', 600.00, 15, 1),
(3, 'T-Shirt', 20.00, 50, 2),
(4, 'Jeans', 40.00, 30, 2),
(5, 'SQL Book', 30.00, 25, 3);

---

## Insert Orders

INSERT INTO Orders VALUES
(1, 1, '2024-04-01', 'Shipped'),
(2, 2, '2024-04-02', 'Pending'),
(3, 1, '2024-04-10', 'Delivered'),
(4, 3, '2024-04-12', 'Shipped');

---

## Insert Order Items

INSERT INTO Order_Items VALUES
(1, 1, 1, 1, 800.00),
(2, 1, 3, 2, 20.00),
(3, 2, 2, 1, 600.00),
(4, 3, 5, 3, 30.00),
(5, 4, 4, 1, 40.00),
(6, 4, 3, 2, 20.00);