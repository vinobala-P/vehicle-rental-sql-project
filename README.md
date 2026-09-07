#  Vehicle Rental Database — SQL Project

A relational database project simulating a vehicle rental business — schema design, sample data generation, and 40 SQL practice problems (easy to hard).

##  Overview

This project models a complete vehicle rental system with **10 interconnected tables**, covering branches, employees, customers, vehicles, bookings, payments, reviews, maintenance, and insurance records. It includes over **21,000 realistic sample records** and a curated set of practice SQL queries ranging from beginner to advanced.

##  Database Structure

| Table | Description |
|---|---|
| `Branches` | Rental office locations |
| `Vehicle_Categories` | Vehicle type classifications (Two Wheelers, Cars, Commercial, etc.) |
| `Employees` | Staff records linked to branches |
| `Customers` | Customer details |
| `Vehicles` | Vehicle inventory linked to categories |
| `Bookings` | Central transaction table — links customers, vehicles, and branches |
| `Payments` | Payment records linked to bookings |
| `Reviews` | Customer feedback linked to bookings |
| `Maintenance` | Vehicle service/repair history |
| `Insurance` | Vehicle insurance policy records |

**Bookings** is the central table, connecting Customers, Vehicles, Branches, Payments, and Reviews. **Vehicles** is the second most connected table, tying together Categories, Maintenance, and Insurance.

##  Project Structure

```
vehicle-rental-sql-project/
├── schema/     → CREATE TABLE statements for all 10 tables
├── data/       → INSERT statements with 21,000+ sample records
├── queries/    → 40 practice SQL questions with answers (beginner to advanced)
└── README.md
```

##  Tech Stack

- **Database:** MySQL 8.x

##  Setup Instructions

1. Create a database:
   ```sql
   CREATE DATABASE vehicle_rental_db;
   USE vehicle_rental_db;
   ```
2. Run the schema file to create all tables:
   ```
   Run the .sql file inside /schema
   ```
3. Run the data file to populate the tables:
   ```
   Run the .sql file inside /data
   ```
4. Explore the practice queries in `/queries` to test your SQL skills against the dataset — from basic `SELECT` statements to window functions and CTEs.

##  Sample Query

Find the customer who has spent the most money in total across all bookings:

```sql
SELECT c.customer_id, c.first_name, c.last_name, SUM(p.payment_amount) AS total_spent
FROM Customers c
JOIN Bookings b ON c.customer_id = b.customer_id
JOIN Payments p ON b.booking_id = p.booking_id
GROUP BY c.customer_id, c.first_name, c.last_name
ORDER BY total_spent DESC
LIMIT 1;
```

## 🎯 What This Project Demonstrates

- Relational database design with primary/foreign key relationships
- Realistic sample data generation
- SQL querying across joins, aggregations, subqueries, window functions, and CTEs
