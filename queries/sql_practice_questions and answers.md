# Vehicle Rental Database — SQL Practice Questions & Answers

Based on your 10 tables: Branches, Vehicle_Categories, Employees, Customers, Vehicles, Bookings, Payments, Reviews, Maintenance, Insurance.

---

## 🟢 BEGINNER (15 Questions)

1. List all branch names from the `Branches` table.
```sql
SELECT branch_name FROM Branches;
```

2. Display all customers from the `Customers` table who live in the city 'Chennai'.
```sql
SELECT * FROM Customers
WHERE city = 'Chennai';
```

3. Show all vehicle names and their category IDs from the `Vehicles` table.
```sql
SELECT vehicle_name, category_id FROM Vehicles;
```

4. Find all employees whose salary is greater than 30000.
```sql
SELECT * FROM Employees
WHERE salary > 30000;
```

5. List all bookings with a `booking_status` of 'Completed'.
```sql
SELECT * FROM Bookings
WHERE booking_status = 'Completed';
```

6. Display all customers sorted by `registration_date` in descending order.
```sql
SELECT * FROM Customers
ORDER BY registration_date DESC;
```

7. Show the `payment_method` and `payment_amount` for all payments with status 'Success'.
```sql
SELECT payment_method, payment_amount FROM Payments
WHERE payment_status = 'Success';
```

8. Find all vehicles belonging to `category_id` = 3.
```sql
SELECT * FROM Vehicles
WHERE category_id = 3;
```

9. List all reviews written after '2026-01-01', showing the review text and review date.
```sql
SELECT review_text, review_date FROM Reviews
WHERE review_date > '2026-01-01';
```

10. Show the top 10 customers with the highest `customer_id`.
```sql
SELECT * FROM Customers
ORDER BY customer_id DESC
LIMIT 10;
```

11. Find all employees who work at `branch_id` = 1.
```sql
SELECT * FROM Employees
WHERE branch_id = 1;
```

12. Display the `vehicle_id` and `maintenance_cost` for all maintenance records above 5000.
```sql
SELECT vehicle_id, maintenance_cost FROM Maintenance
WHERE maintenance_cost > 5000;
```

13. List all insurance policies expiring (`end_date`) in the year 2026.
```sql
SELECT * FROM Insurance
WHERE YEAR(end_date) = 2026;
```

14. Find all bookings made in the last 30 days (use `booking_date`).
```sql
SELECT * FROM Bookings
WHERE booking_date >= CURDATE() - INTERVAL 30 DAY;
```

15. Show all distinct `job_role` values from the `Employees` table.
```sql
SELECT DISTINCT job_role FROM Employees;
```

---

## 🟡 INTERMEDIATE (15 Questions)

1. Find the total number of bookings made at each branch.
```sql
SELECT branch_id, COUNT(*) AS total_bookings
FROM Bookings
GROUP BY branch_id;
```

2. Display the total revenue generated from all bookings.
```sql
SELECT SUM(total_amount) AS total_revenue
FROM Bookings;
```

3. List each vehicle category along with the count of vehicles in that category.
```sql
SELECT vc.category_name, COUNT(v.vehicle_id) AS vehicle_count
FROM Vehicle_Categories vc
LEFT JOIN Vehicles v ON vc.category_id = v.category_id
GROUP BY vc.category_name;
```

4. Show the average `payment_amount` grouped by `payment_method`.
```sql
SELECT payment_method, AVG(payment_amount) AS avg_amount
FROM Payments
GROUP BY payment_method;
```

5. Find the top 5 customers who have made the most bookings.
```sql
SELECT customer_id, COUNT(*) AS booking_count
FROM Bookings
GROUP BY customer_id
ORDER BY booking_count DESC
LIMIT 5;
```

6. List all bookings along with the customer's first and last name.
```sql
SELECT b.booking_id, c.first_name, c.last_name, b.booking_status
FROM Bookings b
JOIN Customers c ON b.customer_id = c.customer_id;
```

7. Display each vehicle's name along with the total maintenance cost incurred.
```sql
SELECT v.vehicle_name, SUM(m.maintenance_cost) AS total_maintenance_cost
FROM Vehicles v
JOIN Maintenance m ON v.vehicle_id = m.vehicle_id
GROUP BY v.vehicle_name;
```

8. Find the number of reviews written each year, sorted by year.
```sql
SELECT YEAR(review_date) AS review_year, COUNT(*) AS review_count
FROM Reviews
GROUP BY YEAR(review_date)
ORDER BY review_year;
```

9. List all branches along with the number of employees working there.
```sql
SELECT br.branch_name, COUNT(e.employee_id) AS employee_count
FROM Branches br
LEFT JOIN Employees e ON br.branch_id = e.branch_id
GROUP BY br.branch_name;
```

10. Show all customers who have never made a booking.
```sql
SELECT * FROM Customers
WHERE customer_id NOT IN (SELECT customer_id FROM Bookings);
```

11. Find the total payment amount collected per branch.
```sql
SELECT br.branch_name, SUM(p.payment_amount) AS total_collected
FROM Payments p
JOIN Bookings b ON p.booking_id = b.booking_id
JOIN Branches br ON b.branch_id = br.branch_id
GROUP BY br.branch_name;
```

12. Display the average `total_amount` of bookings for each `booking_status`.
```sql
SELECT booking_status, AVG(total_amount) AS avg_amount
FROM Bookings
GROUP BY booking_status;
```

13. List vehicles that have had more than 3 maintenance records.
```sql
SELECT vehicle_id, COUNT(*) AS maintenance_count
FROM Maintenance
GROUP BY vehicle_id
HAVING COUNT(*) > 3;
```

14. Find all employees hired in the last 2 years, along with their branch name.
```sql
SELECT e.employee_name, e.hire_date, br.branch_name
FROM Employees e
JOIN Branches br ON e.branch_id = br.branch_id
WHERE e.hire_date >= CURDATE() - INTERVAL 2 YEAR;
```

15. Show the number of bookings per month.
```sql
SELECT YEAR(booking_date) AS yr, MONTH(booking_date) AS mo, COUNT(*) AS booking_count
FROM Bookings
GROUP BY YEAR(booking_date), MONTH(booking_date)
ORDER BY yr, mo;
```

---

## 🔴 HARD (10 Questions)

1. Find the customer who has spent the most money in total across all bookings.
```sql
SELECT c.customer_id, c.first_name, c.last_name, SUM(p.payment_amount) AS total_spent
FROM Customers c
JOIN Bookings b ON c.customer_id = b.customer_id
JOIN Payments p ON b.booking_id = p.booking_id
GROUP BY c.customer_id, c.first_name, c.last_name
ORDER BY total_spent DESC
LIMIT 1;
```

2. For each branch, find the vehicle category that has been booked the most times.
```sql
SELECT branch_id, category_id, booking_count
FROM (
    SELECT b.branch_id, v.category_id, COUNT(*) AS booking_count,
           RANK() OVER (PARTITION BY b.branch_id ORDER BY COUNT(*) DESC) AS rnk
    FROM Bookings b
    JOIN Vehicles v ON b.vehicle_id = v.vehicle_id
    GROUP BY b.branch_id, v.category_id
) ranked
WHERE rnk = 1;
```

3. Identify vehicles that generated more revenue than the average revenue per vehicle across all vehicles.
```sql
SELECT vehicle_id, SUM(total_amount) AS vehicle_revenue
FROM Bookings
GROUP BY vehicle_id
HAVING SUM(total_amount) > (
    SELECT AVG(vehicle_total)
    FROM (
        SELECT SUM(total_amount) AS vehicle_total
        FROM Bookings
        GROUP BY vehicle_id
    ) t
);
```

4. Find the top 3 branches by total revenue, along with their rank.
```sql
SELECT branch_id, total_revenue, RANK() OVER (ORDER BY total_revenue DESC) AS branch_rank
FROM (
    SELECT branch_id, SUM(total_amount) AS total_revenue
    FROM Bookings
    GROUP BY branch_id
) t
LIMIT 3;
```

5. List customers who booked vehicles from more than one branch.
```sql
SELECT customer_id, COUNT(DISTINCT branch_id) AS branch_count
FROM Bookings
GROUP BY customer_id
HAVING COUNT(DISTINCT branch_id) > 1;
```

6. Find the month-over-month growth in total booking revenue.
```sql
SELECT yr, mo, revenue,
       revenue - LAG(revenue) OVER (ORDER BY yr, mo) AS mom_growth
FROM (
    SELECT YEAR(booking_date) AS yr, MONTH(booking_date) AS mo, SUM(total_amount) AS revenue
    FROM Bookings
    GROUP BY YEAR(booking_date), MONTH(booking_date)
) monthly
ORDER BY yr, mo;
```

7. Identify vehicles currently under a valid insurance policy but with no maintenance in the last 6 months.
```sql
SELECT i.vehicle_id
FROM Insurance i
WHERE CURDATE() BETWEEN i.start_date AND i.end_date
AND i.vehicle_id NOT IN (
    SELECT vehicle_id FROM Maintenance
    WHERE maintenance_date >= CURDATE() - INTERVAL 6 MONTH
);
```

8. Find the number of reviews and average review length (characters) per employee's branch.
```sql
SELECT br.branch_name,
       COUNT(r.review_id) AS review_count,
       AVG(LENGTH(r.review_text)) AS avg_review_length
FROM Reviews r
JOIN Bookings b ON r.booking_id = b.booking_id
JOIN Branches br ON b.branch_id = br.branch_id
GROUP BY br.branch_name;
```

9. Using a subquery, find all customers whose total booking amount is above the overall average customer spend.
```sql
SELECT customer_id, SUM(total_amount) AS customer_spend
FROM Bookings
GROUP BY customer_id
HAVING SUM(total_amount) > (
    SELECT AVG(customer_total)
    FROM (
        SELECT SUM(total_amount) AS customer_total
        FROM Bookings
        GROUP BY customer_id
    ) t
);
```

10. Using a CTE, rank vehicles by total revenue within each category, and return only the top 2 vehicles per category.
```sql
WITH vehicle_revenue AS (
    SELECT v.vehicle_id, v.vehicle_name, v.category_id, SUM(b.total_amount) AS revenue
    FROM Vehicles v
    JOIN Bookings b ON v.vehicle_id = b.vehicle_id
    GROUP BY v.vehicle_id, v.vehicle_name, v.category_id
),
ranked AS (
    SELECT *, RANK() OVER (PARTITION BY category_id ORDER BY revenue DESC) AS rnk
    FROM vehicle_revenue
)
SELECT * FROM ranked
WHERE rnk <= 2;
```

---

*All queries assume MySQL 8.x syntax (for `RANK()`, `LAG()`, and CTEs). If you're on an older MySQL version without window function support, let me know and I can rewrite those.*
