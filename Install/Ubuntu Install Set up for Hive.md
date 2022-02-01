## <span id='top'>Set up for Hive</span>

Install Hadoop. 

[[Install MySQL]](#install)  
[[Setup MySQL]](#setup)  

In order to run `hive`, you need the following set. If any of them is missing, please proceed to install them (Nalawade). 

- `MySql`
- `Hadoop`
- `Hive`
- `Java`

<br>
<br>

## <span id='install'>Install `MySQL`</span>

[[Top]](#top)

1. `sudo apt update`
2. Install `MySQL`
3. Launch `Hive Metastore` and `Hive Database`.

<br>

### 1. `sudo apt update`

    20 packages can be upgraded. Run 'apt list --upgradable' to see them.

<br>

### 2. `sudo apt install -y mysql-server`

    mysqld will log errors to /var/log/mysql/error.log
    mysqld is running as pid 9252
    Created symlink /etc/systemd/system/multi-user.target.wants/mysql.service → /lib
    /systemd/system/mysql.service.

<br>
<br>

## <span id='setup'>Set up `MySQL`</span>

[[Top]](#top)

    sudo mysql_secure_installation utility

<br>

Configure password and security-related options. 

    Securing the MySQL server deployment.

    Connecting to MySQL using a blank password.

<br>

`VALIDATE PASSWORD COMPONENT`

    VALIDATE PASSWORD COMPONENT can be used to test passwords
    and improve security. It checks the strength of password
    and allows the users to set only those passwords which are
    secure enough. Would you like to setup VALIDATE PASSWORD component?

    Press y|Y for Yes, any other key for No: y

    There are three levels of password validation policy:

    LOW    Length >= 8
    MEDIUM Length >= 8, numeric, mixed case, and special characters
    STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

    Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
    Please set the password for root here.

    New password: 

    Re-enter new password: 

<br>

Anonymous user option: 

    Estimated strength of the password: 50 
    Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y

    By default, a MySQL installation has an anonymous user,
    allowing anyone to log into MySQL without having to have
    a user account created for them. This is intended only for
    testing, and to make the installation go a bit smoother.
    You should remove them before moving into a production
    environment.

    Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
    Success.

<br>

Remote root login option: 

    Normally, root should only be allowed to connect from
    'localhost'. This ensures that someone cannot guess at
    the root password from the network.

    Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n

    ... skipping.

<br>

`Test db` option: 

    By default, MySQL comes with a database named 'test' that
    anyone can access. This is also intended only for testing,
    and should be removed before moving into a production
    environment.


    Remove test database and access to it? (Press y|Y for Yes, any other key for No) : n

    ... skipping.
    Reloading the privilege tables will ensure that all changes
    made so far will take effect immediately.

    Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
    Success.

    All done! 

<br>
<br>

### 3. Remote Access configuration

    sudo ufw enable

    Firewall is active and enabled on system startup

    
    sudo ufw allow mysql
    
    Rule added
    Rule added (v6)

<br>

### 4. Launch `MySQL`

    sudo systemctl start mysql
    sudo systemctl enable mysql

<br>
<br>


### References 

- Arti Nalawade (2016) java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient https://stackoverflow.com/questions/35449274/java-lang-runtimeexception-unable-to-instantiate-org-apache-hadoop-hive-ql-meta