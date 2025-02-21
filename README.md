# MySUI notes

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

