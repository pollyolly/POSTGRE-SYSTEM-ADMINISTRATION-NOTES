### INSTALL POSTGRE
```
# Create the file repository configuration:
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Import the repository signing key:
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# Update the package lists:
sudo apt-get update

# Install the latest version of PostgreSQL.
# If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
sudo apt-get -y install postgresql
```
### CONFIGURATION FILE
```
/etc/postgresql/10/main/postgresql.conf 
```
### LOGIN AS SUPER USER
```
$sudo -u postgres psql
postgres=# select version(); --Check Version
```
### POSTGRE COMMAND
```
// List Databases
postgres=# \l

// Create Database
postgres=# CREATE DATABASE mytest;

// Connect / Use Database
postgres=# \c mytest

// Display all tables
mytest=# \dt

// Create Enumeration type
mytest=# CREATE TYPE cat_enum AS ENUM ('coffee', 'tea');

// Display all Enumeration type
mytest=# \dT+

// Create a new table.
mytest=# CREATE TABLE IF NOT EXISTS cafe (
  id SERIAL PRIMARY KEY,        -- AUTO_INCREMENT integer, as primary key
  category cat_enum NOT NULL,   -- Use the enum type defined earlier
  name VARCHAR(50) NOT NULL,    -- Variable-length string of up to 50 characters
  price NUMERIC(5,2) NOT NULL,  -- 5 digits total, with 2 decimal places
  last_update DATE              -- 'YYYY-MM-DD'
);
NOTICE:  CREATE TABLE will create implicit sequence "cafe_id_seq" for serial column "cafe.id"
NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "cafe_pkey" for table "cafe"

// Display Particular Table
mytest=# \d+ cafe

// Insert
mytest=# INSERT INTO cafe (category, name, price) VALUES
  ('tea', 'Wulong Tea', 2.89);

// Query
mytest=# SELECT * FROM cafe;

// Update
mytest=# UPDATE cafe SET price = price * 1.1 WHERE category = 'tea';

// Delete
mytest=# DELETE FROM cafe WHERE id = 6;

// Quit
mytest=# \q
```
### REFERENCES
[Getting Started with PostgreSQL](https://www3.ntu.edu.sg/home/ehchua/programming/sql/PostgreSQL_GetStarted.html)
