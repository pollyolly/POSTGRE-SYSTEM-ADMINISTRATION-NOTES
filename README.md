### INSTALL POSTGRE
```bash
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
```vim
/etc/postgresql/10/main/postgresql.conf 
```
### LOGIN AS SUPER USER
```vim
$sudo -u postgres psql
postgres=# select version(); --Check Version
```
### POSTGRE COMMAND
#### CREATE USER AND CREATE DB
```vim
// Create a new PostgreSQL user called testuser, allow user to login, but NOT creating databases
$ sudo -u postgres createuser --login --pwprompt testuser

# Create a new database called testdb, owned by testuser.
$ sudo -u postgres createdb --owner=testuser testdb
```
#### CREATE DATABASE ROLE
Role can function as a user or a group (used for database connection)
```vim
mytest=# CREATE ROLE testrole WITH LOGIN PASSWORD 'password'; //Creating database user
mytest=# ALTER ROLE testrole CREATEDB; //Allow to Create DB
mytest=#\du //Check available Roles

Roles attributes:
CREATEDB = allow create db
SUPERUSER = allow query
REPLICATION
CREATEROLE
```
[Postgres Roles](https://www.postgresql.org/docs/current/sql-alterrole.html)

#### Permissions
```sql
-- Remove Access to Query
REVOKE ALL PRIVILEGES ON DATABASE dianna_db FROM other_username;
REVOKE ALL ON DATABASE dianna_db FROM PUBLIC;
-- Grant all privileges to user
GRANT ALL PRIVILEGES ON DATABASE "dianna_db" to dianna;
```
#### SQL
```vim
//Use Database
postgres=#\c sample_database

//Show all Database
postgres=#\list

// List Databases
postgres=# \l

// Create Database
postgres=# CREATE DATABASE mytestdb;

//Use Database or Connect to Database
postgres=# \c mytest

// Display all tables
mytest=# \dt

// Create Enumeration type
mytest=# CREATE TYPE cat_enum AS ENUM ('coffee', 'tea');

// Display all Enumeration type
mytest=# \dT+

// Display all Roles
mytest=#\du

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

//Check Connection Info (Port, User, Database)
mytest=#\conninfo
```
### COMMON DATA TYPES 
```vim
1. INT, SMALLINT: whole number. There is no UNSIGNED attribute in PostgreSQL.
2. SERIAL: auto-increment integer (AUTO_INCREMENT in MySQL).
3. REAL, DOUBLE: single and double precision floating-point number.
4. CHAR(n) and VARCHAR(n)
5. NUMERIC(m,n): decimal number with m total digits and n decimal places (DECIMAL(m,n) in MySQL).
6. DATE, TIME, TIMESTAMP, INTERVAL: date and time.
```
### SET PASSWORD FOR POSTGRES
```vim
-- Login in to server via "psql" with user "postgres"
$ sudo -u postgres psql
......
 
-- Change password for current user "postgres"
postgres=# \password postgres
Enter new password: xxxx
Enter it again: xxxx
  
-- Display the user table
postgres=# SELECT * FROM pg_user;
 usename  | usesysid | usecreatedb | usesuper | usecatupd | userepl |  passwd  | valuntil | useconfig 
----------+----------+-------------+----------+-----------+---------+----------+----------+-----------
 postgres |       10 | t           | t        | t         | t       | ******** |          |
 
-- Quit
postgres=# \q
```
### CREATE GROUP AND USER
```vim
-- Create a login user role
CREATE ROLE user1 LOGIN PASSWORD 'xxxx' CREATEDB VALID UNTIL 'infinity';

-- Create a login superuser role
CREATE ROLE user2 LOGIN PASSWORD 'xxxx' SUPERUSER VALID UNTIL '2019-12-31';

-- Create a group role
CREATE ROLE group1 INHERIT;
-- Add a user (or group) to this group
GRANT group1 TO user1;
```
### BACKUP AND RESTORE
```vim
-- Create a compressed backup for a database
pg_dump -h localhost -p 5432 -U username -F c -b -v -f mydatabase.backup mydatabase

-- Create a plain-text backup for a database, including the CREATE DATABASE
pg_dump -h localhost -p 5432 -U username -C -F p -b -v -f mydatabase.backup.sql mydatabase
```
```vim
-- Run SQL script
$ psql -U username -f filename.sql
```
### REMOTE SETUP
[PostgreSQL-Remote-connection-with-pgadmin-on-a-virtual-private-server-ubuntu](https://medium.com/@johnmark_76235/postgresql-remote-connection-with-pgadmin-on-a-virtual-private-server-ubuntu-f82bcc9e197c)

### BACKUP
[PostgreSQL Backup](https://medium.com/@johnmark_76235/postgresql-backup-5b2ca6956410)

### TROUBLESHOOTING
#### Set/Allow non-default user to login
```
/etc/postgresql/10/main/pg_hba.conf
# TYPE  DATABASE    USER        ADDRESS          METHOD
local   testdb      testuser                     md5
```


### REFERENCES
[Getting Started with PostgreSQL](https://www3.ntu.edu.sg/home/ehchua/programming/sql/PostgreSQL_GetStarted.html)
