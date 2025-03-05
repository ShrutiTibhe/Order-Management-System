# Sales Database Creation and Population - README

This document outlines the SQL commands used to create and populate a sales database with three tables: `SALESMEN`, `CUSTOMER`, and `ORDERS`. It details the database structure, constraints, data insertion, and basic data retrieval.

## Database Structure

The database consists of the following tables:

- **SALESMEN**: Stores information about salesmen, including their ID, name, city, and commission.
- **CUSTOMER**: Stores information about customers, including their ID, name, city, rating, and the ID of their assigned salesman.
- **ORDERS**: Stores information about orders, including the order ID, amount, date, customer ID, and salesman ID.

## Tables and Constraints

### SALESMEN Table

```sql
CREATE TABLE SALESMEN (
    SNUM VARCHAR2(6),
    SNAME VARCHAR2(20) NOT NULL,
    CITY VARCHAR2(15),
    COMM NUMBER(5,2),
    CONSTRAINT pk_salesmen PRIMARY KEY (SNUM),
    CONSTRAINT chk_snum CHECK (SUBSTR(SNUM, 1, 1) = 'S')
);
```
- `SNUM` (Salesman Number): Primary key, must start with 'S'.
- `SNAME` (Salesman Name): Cannot be null.
- `CITY` (Salesman City).
- `COMM` (Commission): Numeric value with 5 digits and 2 decimal places.

### CUSTOMER Table

```sql
CREATE TABLE CUSTOMER (
    CNUM VARCHAR2(6),
    CNAME VARCHAR2(20) NOT NULL,
    CITY VARCHAR2(15),
    RATING NUMBER(5),
    SNUM VARCHAR2(6),
    CONSTRAINT pk_customer PRIMARY KEY (CNUM),
    CONSTRAINT chk_cnum CHECK (SUBSTR(CNUM, 1, 1) = 'C'),
    CONSTRAINT fk_salesmen FOREIGN KEY (SNUM) REFERENCES SALESMEN(SNUM)
);
```
- `CNUM` (Customer Number): Primary key, must start with 'C'.
- `CNAME` (Customer Name): Cannot be null.
- `CITY` (Customer City).
- `RATING` (Customer Rating).
- `SNUM` (Salesman Number): Foreign key referencing `SALESMEN(SNUM)`.

### ORDERS Table

```sql
CREATE TABLE "ORDER" (
    ONUM VARCHAR2(6),
    AMT NUMBER(10,2) NOT NULL,
    ODATE DATE,
    CNUM VARCHAR2(6),
    SNUM VARCHAR2(6),
    CONSTRAINT pk_order PRIMARY KEY (ONUM),
    CONSTRAINT chk_onum CHECK (SUBSTR(ONUM, 1, 1) = 'O'),
    CONSTRAINT fk_customer FOREIGN KEY (CNUM) REFERENCES CUSTOMER(CNUM),
    CONSTRAINT fk_salesperson FOREIGN KEY (SNUM) REFERENCES SALESMEN(SNUM)
);

RENAME "ORDER" TO ORDERS;
```
- `ONUM` (Order Number): Primary key, must start with 'O'.
- `AMT` (Amount): Cannot be null.
- `ODATE` (Order Date).
- `CNUM` (Customer Number): Foreign key referencing `CUSTOMER(CNUM)`.
- `SNUM` (Salesman Number): Foreign key referencing `SALESMEN(SNUM)`.
- The table was created as `ORDER` and then renamed to `ORDERS`.

## Data Insertion

The following `INSERT` statements were used to populate the tables:

- **SALESMEN**: 7 rows of salesmen data.
- **CUSTOMER**: 7 rows of customer data, including the correct foreign key for salesmen.
- **ORDERS**: 10 rows of order data, including correct foreign keys for customers and salesmen.

## Data Retrieval

The following `SELECT` statements were used to verify the data:

```sql
SELECT * FROM ORDERS;
SELECT * FROM CUSTOMER;
SELECT * FROM SALESMEN;
```

## Commit

The `COMMIT;` command was used to save the changes to the database.

## Observations

- Primary key, foreign key, and check constraints were successfully implemented.
- Data was inserted and retrieved as expected.
- An error of "not enough values" occurred during the first attempt to insert data into the `CUSTOMER` table, which was resolved by adding the `SNUM` value to the insert statement.
- The `ORDER` table was renamed to `ORDERS` to avoid potential reserved word conflicts.
