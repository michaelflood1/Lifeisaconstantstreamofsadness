# MySUI notes


### Change the MySQL Port (on Windows and Linux)
### Windows: Modify the my.ini file (usually in C:\ProgramData\MySQL\MySQL Server X.X)
### Linux: Modify the my.cnf file (usually in /etc/mysql/my.cnf or /etc/my.cnf)
### Example:
### [mysqld]
### port = 3307
### Restart MySQL to apply changes

### Login to MySQL Using a Non-Standard Port
mysql -u username -p -P 3307 -h localhost

### Create Databases
CREATE DATABASE my_database;

### Create Tables
CREATE TABLE my_table (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    created_at TIMESTAMP
);

### Insert, Update, and Delete Data
### Insert Data
INSERT INTO my_table (name, created_at) VALUES ('John Doe', NOW());

### Update Data
UPDATE my_table SET name = 'Jane Doe' WHERE id = 1;

### Delete Data
DELETE FROM my_table WHERE id = 1;

### Create MySQL Users, Roles, and Grant Privileges
### Create User
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';

### Grant Privileges
GRANT ALL PRIVILEGES ON my_database.* TO 'new_user'@'localhost';

### Create Role
CREATE ROLE 'my_role';
GRANT SELECT ON my_database.* TO 'my_role';

### Assign Role to User
GRANT 'my_role' TO 'new_user'@'localhost';

### Troubleshoot Users and Privileges
### Show User Privileges
SHOW GRANTS FOR 'username'@'host';

### Fix Susanna's Privileges
SHOW GRANTS FOR 'susanna'@'localhost';
GRANT SELECT, INSERT, UPDATE ON my_database.* TO 'susanna'@'localhost';

### Import CSV Data Using LOAD DATA INFILE
LOAD DATA INFILE '/path/to/data.csv'
INTO TABLE my_table
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;

### Check the secure_file_priv variable
SHOW VARIABLES LIKE 'secure_file_priv';

### Import SQL Formatted Data from mysqldump File
mysql -u username -p my_database < backup.sql

### Recover from Accidental Data Deletion
### If using InnoDB, you can attempt to recover from the binary log.
### Always have regular backups for recovery.

### Reset the MySQL Root Password (on Windows and Linux)
### Linux:
### 1. Stop MySQL: sudo systemctl stop mysql
### 2. Start in safe mode: sudo mysqld_safe --skip-grant-tables &
### 3. Log in: mysql -u root
### 4. Change password:
###    UPDATE mysql.user SET authentication_string = PASSWORD('new_password') WHERE User = 'root';
###    FLUSH PRIVILEGES;

### Windows:
### 1. Stop MySQL service via Services window.
### 2. Start MySQL with --skip-grant-tables.
### 3. Log in and reset the password using the same SQL commands as above.
### 4. Restart MySQL.

### InnoDB vs MyISAM Table Storage Engines
### InnoDB: Supports transactions, foreign keys, and row-level locking (default).
### MyISAM: Does not support transactions or foreign keys; uses table-level locking.

### Location of MySQL Configuration Files
### Windows: C:\ProgramData\MySQL\MySQL Server X.X\my.ini
### Linux: /etc/mysql/my.cnf, /etc/my.cnf, or /etc/mysql/mysql.conf.d/mysqld.cnf

### Standard Port for MySQL
### Default MySQL port: 3306

### Setup Users, Roles, and Privileges
### Example:
### CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';
### GRANT SELECT, INSERT, UPDATE ON my_database.* TO 'new_user'@'localhost';
### FLUSH PRIVILEGES;

### Cleanup After Resetting the Root Password
### Ensure that --skip-grant-tables is no longer in use after resetting the password.
### Restart MySQL to apply changes.

### secure_file_priv and LOAD DATA INFILE
### secure_file_priv restricts file loading to certain directories.
SHOW VARIABLES LIKE 'secure_file_priv';

### How to Use LOAD DATA INFILE with Different Line Endings
### Windows: LINES TERMINATED BY '\r\n'
### Mac: LINES TERMINATED BY '\r'
### Linux: LINES TERMINATED BY '\n'

### Check Line Endings in Files (Linux and Windows)
### Linux: Use `file filename` to check the line endings.
### Windows: Open the file in a text editor like Notepad++ to check line endings.



1. **`mysql -u root -p`**  
   Connects to MySQL as the root user, prompting for the root password.

2. **`mysql -u root -p -h <ip_address>`**  
   Attempts to connect to MySQL on a remote server specified by `<ip_address>`.

3. **`CREATE USER 'testuser'@'%' IDENTIFIED BY 'testing';`**  
   Creates a new MySQL user `testuser` with password `testing` that can connect from any host.

4. **`SELECT user, host FROM mysql.user;`**  
   Displays a list of users and their associated hosts in the MySQL database.

5. **`mysql -P 3305 -u root -p`**  
   Attempts to connect to MySQL using port 3305 instead of the default port 3306.

6. **`sudo nano /etc/mysql/my.cnf`**  
   Opens the MySQL configuration file (`my.cnf`) in the nano text editor with superuser privileges.

7. **`sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf`**  
   Opens the MySQL server configuration file (`mysqld.cnf`) in nano for editing.

8. **`sudo service mysql restart`**  
   Restarts the MySQL service on a Linux machine to apply configuration changes.

9. **`sudo service mysql status`**  
   Checks the status of the MySQL service to ensure it is running.

10. **`sudo netstat -ltnp`**  
    Displays network connections and listening ports, confirming MySQL is using port 3305.

11. **`mysqld --verbose --help`**  
    Provides a detailed list of MySQL server command-line options and configuration variables.

1. **`python3 --version`**  
   Checks the version of Python installed on the system.

2. **`sudo apt install python3-pip`**  
   Installs the Python package installer (PIP) for managing Python packages.

3. **`python3 -m pip install mysql-connector-python`**  
   Installs the MySQL connector package for Python, enabling Python to communicate with MySQL.

4. **Python Script:**
   ```python
   import mysql.connector
   mydb = mysql.connector.connect(
       host="localhost",
       user="root",
       passwd="yourRootPassword"
   )
   mycursor = mydb.cursor()
   mycursor.execute("SHOW VARIABLES like 'version'")
   print(mydb)
   myresult = mycursor.fetchall()
   for row in myresult:
       print(row)
   ```
   - **Explanation**: The script connects to the MySQL database, executes a query to show the MySQL version, and prints the results.

5. **`python3 mysql-connect.py`**  
   Runs the Python script that connects to the MySQL database and outputs the result of the query.

1. **Stop MySQL Windows Service**  
   Stops the MySQL service so it can be restarted with the new settings:  
   `net stop MySQL80`

2. **Create `changeRootPassword.sql` File**  
   A file is created with the SQL command to change the root password:  
   `ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPassword';`

3. **Run MySQL with Init File**  
   Runs MySQL with the init file to apply the password change:  
   `mysqld --defaults-file="C:\ProgramData\MySQL\MySQL Server 8.0\my.ini" --init-file=C:\temp\ChangeRootPassword.sql`

4. **Restart MySQL Service**  
   Restarts the MySQL service to apply the password change:  
   `net start MySQL80`

5. **Try New Password**  
   Log into MySQL to check if the new password works:  
   `mysql -u root -p`

---

6. **Stop MySQL Service (Linux)**  
   Stops the MySQL service on Linux:  
   `sudo service mysql stop`

7. **Create Directory and Set Permissions**  
   Creates necessary directories and sets correct permissions:  
   `sudo mkdir /var/run/mysqld/`  
   `sudo chown mysql /var/run/mysqld/`

8. **Run MySQL in Safe Mode**  
   Starts MySQL in safe mode without needing a password:  
   `sudo mysqld_safe --skip-grant-tables --skip-networking --user=mysql &`

9. **Change Root Password**  
   Log into MySQL and set a new password for root:  
   `mysql`  
   `ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPassword';`  
   `FLUSH PRIVILEGES;`

10. **Kill Safe Mode MySQL Process**  
    Stops the safe mode MySQL process:  
    `sudo killall mysqld`

11. **Start MySQL Normally**  
    Starts MySQL normally after safe mode is stopped:  
    `sudo service mysql start`

12. **Confirm New Password**  
    Log into MySQL again to confirm the password change:  
    `mysql -u root -p`

```

insert into employees.employees (emp_no, birth_date, first_name, last_name, gender, hire_date)
select eb.emp_no, eb.birth_date, eb.first_name, eb.last_name, eb.gender, eb.hire_date
from employees_backup.employess eb
left join employees.employees e on eb.emp_no = e.emp_no
where e.emp_no is null;



Load Data infile 'C:\\ProgramData\\MySQL\\MySQL Server 8.0\\Uploads\\words.txt'
into table dictionary.word
lines terminated by '\r\n'
(word)
```


Grant Role to User
Grants a specific role to a user:

    GRANT 'role_name' TO 'username'@'host';

Grant Multiple Roles to User
Grants multiple roles to a user at once:

    GRANT 'role1', 'role2' TO 'username'@'host';

Grant Privileges Along with a Role
You can grant both a role and specific privileges at the same time:

    GRANT 'role_name', ALL PRIVILEGES ON database_name.* TO 'username'@'host';

Set Default Role for a User
Sets a default role for the user:

    SET DEFAULT ROLE 'role_name' TO 'username'@'host';


Show Granted Roles for a User
To see which roles are assigned to a user:

    SHOW GRANTS FOR 'username'@'host';

Revoke Role from User
To remove a role from a user:

    REVOKE 'role_name' FROM 'username'@'host';

-- Creating the 'address_type' table
CREATE TABLE address_type (
    address_type_id INT NOT NULL AUTO_INCREMENT,
    type VARCHAR(15) NOT NULL,
    PRIMARY KEY (address_type_id)
);

-- Creating the 'address' table
CREATE TABLE address (
    address_id INT NOT NULL AUTO_INCREMENT,
    house_number VARCHAR(45) NOT NULL,
    street_name VARCHAR(45) NOT NULL,
    city VARCHAR(45) NOT NULL,
    province VARCHAR(45) NOT NULL,
    country VARCHAR(45) NOT NULL,
    address_frn_address_type_id INT NOT NULL,
    PRIMARY KEY (address_id),
    FOREIGN KEY (address_frn_address_type_id) REFERENCES address_type(address_type_id)
);

-- Creating the 'contact' table
CREATE TABLE contact (
    contact_id INT NOT NULL AUTO_INCREMENT,
    first_name VARCHAR(45) NOT NULL,
    last_name VARCHAR(45),
    birth_date DATETIME,
    PRIMARY KEY (contact_id)
);

-- Creating the 'contact_address' table
CREATE TABLE contact_address (
    contact_address_id INT NOT NULL AUTO_INCREMENT,
    contact_frn_contact_id INT NOT NULL,
    contact_frn_address_id INT NOT NULL,
    PRIMARY KEY (contact_address_id),
    FOREIGN KEY (contact_frn_contact_id) REFERENCES contact(contact_id),
    FOREIGN KEY (contact_frn_address_id) REFERENCES address(address_id)
);

-- Creating the 'event' table
CREATE TABLE event (
    event_id INT NOT NULL AUTO_INCREMENT,
    title VARCHAR(45) NOT NULL,
    description VARCHAR(500),
    start_time DATETIME NOT NULL,
    end_time DATETIME,
    event_frn_host_contact_id INT NOT NULL,
    event_frn_address_id INT NOT NULL,
    PRIMARY KEY (event_id),
    FOREIGN KEY (event_frn_host_contact_id) REFERENCES contact(contact_id),
    FOREIGN KEY (event_frn_address_id) REFERENCES address(address_id)
);

-- Creating the 'event_contact' table
CREATE TABLE event_contact (
    event_contact_id INT NOT NULL AUTO_INCREMENT,
    event_contact_frn_event_id INT NOT NULL,
    event_contact_frn_contact_id INT NOT NULL,
    PRIMARY KEY (event_contact_id),
    FOREIGN KEY (event_contact_frn_event_id) REFERENCES event(event_id),
    FOREIGN KEY (event_contact_frn_contact_id) REFERENCES contact(contact_id)
);

-- Creating the 'address_type' table
CREATE TABLE address_type (
    address_type_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    type VARCHAR(15) NOT NULL
);

-- Creating the 'address' table
CREATE TABLE address (
    address_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    house_number VARCHAR(45) NOT NULL,
    street_name VARCHAR(45) NOT NULL,
    city VARCHAR(45) NOT NULL,
    province VARCHAR(45) NOT NULL,
    country VARCHAR(45) NOT NULL,
    frn_address_type_id INT NOT NULL,
    FOREIGN KEY (frn_address_type_id) REFERENCES address_type(address_type_id)
);

-- Creating the 'contact' table
CREATE TABLE contact (
    contact_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(45) NOT NULL,
    last_name VARCHAR(45) DEFAULT NULL,
    birth_date DATETIME DEFAULT NULL
);

-- Creating the 'event' table
CREATE TABLE event (
    event_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(45) NOT NULL,
    description VARCHAR(500) DEFAULT NULL,
    start_time DATETIME NOT NULL,
    end_time DATETIME DEFAULT NULL,
    frn_host_contact_id INT NOT NULL,
    frn_address_id INT NOT NULL,
    FOREIGN KEY (frn_host_contact_id) REFERENCES contact(contact_id),
    FOREIGN KEY (frn_address_id) REFERENCES address(address_id)
);

-- Creating the 'event_contact' table
CREATE TABLE event_contact (
    event_contact_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    frn_event_id INT NOT NULL,
    frn_contact_id INT NOT NULL,
    FOREIGN KEY (frn_event_id) REFERENCES event(event_id),
    FOREIGN KEY (frn_contact_id) REFERENCES contact(contact_id)
);

-- Creating the 'contact_address' table
CREATE TABLE contact_address (
    contact_address_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    frn_contact_id INT NOT NULL,
    frn_address_id INT NOT NULL,
    FOREIGN KEY (frn_contact_id) REFERENCES contact(contact_id),
    FOREIGN KEY (frn_address_id) REFERENCES address(address_id)
);

-- Inserting data into 'address_type'
INSERT INTO address_type (type) VALUES
('work'),
('home'),
('public');

-- Inserting data into 'address'
INSERT INTO address (house_number, street_name, city, province, country, frn_address_type_id) VALUES
('741', 'Heather St', 'Richmond', 'BC', 'Canada', 2),
('10223', 'Shell St', 'Richmond', 'BC', 'Canada', 2),
('#105 887', ' River Rd', 'Delta', 'BC', 'Canada', 2),
('6088', 'Main St', 'Burnaby', 'BC', 'Canada', 3);

-- Inserting data into 'contact'
INSERT INTO contact (first_name, last_name, birth_date) VALUES
('Joe', 'Sorenson', '1997-05-22 0:00'),
('Sara', 'Tortia', '1996-01-10 0:00'),
('Lia', 'Tortia', '1994-08-09 0:00'),
('Michael', '{Park', '1995-06-23 0:00');

-- Inserting data into 'event'
INSERT INTO event (title, description, start_time, end_time, frn_host_contact_id, frn_address_id) VALUES
('Spring Break Kickoff', 'Pool party at the Tortia\'s', '2018-03-15 18:00:00', '2018-03-15 23:50:00', 2, 2);

-- Inserting data into 'event_contact'
INSERT INTO event_contact (frn_event_id, frn_contact_id) VALUES
(1, 1),
(1, 2),
(1, 3),
(1, 4);

-- Inserting data into 'contact_address'
INSERT INTO contact_address (frn_contact_id, frn_address_id) VALUES
(1, 1),
(2, 2),
(3,2),
(4, 3);
