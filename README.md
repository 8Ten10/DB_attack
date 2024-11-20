# DB_attack
DB attack &amp; Exploit

cat /etc/os-release  =>  "Ubuntu 19.10"

Clone (with git) and Deploy https://github.com/8Ten10/deploytools.git

Configure Nginx =>

do : sudo vim /etc/nginx/sites-available/nwit291.com

Add these lines and save
```
server {
    listen 80;
    server_name nwit291.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /home/op/app/static/;
        autoindex on;  # Enable directory listing for debugging
    }
}

```
Do: sudo ln -s /etc/nginx/sites-available/nwit291.com /etc/nginx/sites-enabled/

do : sudo systemctl enable nginx

do: sudo nginx -t  (to make sure nginx is well configured)




Configure Database:

Do : sudo mysql_secure_installation

and configure as folow:
```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

Do sudo mysql -u root -p and enter the root password you previously configured

Paste the folowing to create a database
```
CREATE DATABASE nwit291;
CREATE USER 'Admin'@'localhost' IDENTIFIED BY 'password1234';
GRANT ALL PRIVILEGES ON nwit291_db.* TO 'Admin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Then create the __users__ table with:

do: mysql -u Admin -p then enter your password

Once in Mariadb:

do: USE nwit291;

Then paste and run:
```
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    address VARCHAR(255),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(32)
);
```
MAke sure nginx is running again with

sudo systemctl restart nginx

sudo nginx -t

Clone and Deploy https://github.com/8Ten10/nwit291.com.git

Ensure you are in the correct directory where run.py is located (ls) when you run the following commands.

do: cd app/
do: python3 -m venv venv
do: source venv/bin/activate
do: pip install wheel flask flask-mysqldb
do: python3 run.py

Now tun on your windows workstation to access the server

First of all, Open and edit Windows host file cause there is no dns server. Manually Adding our server in the host file to resolve dns requests to.from the server is the easiest way to get things done.

The host file is located in __C:\Windows\System32\drivers\etc__

Go all the way to the bottom and add your server ip address (do `ip a` to get it) + the server domain name (in this case nwit291.com)
```
192.168.7.145 nwit291.com
```
Open your browser and type nwit291.com, you should have access to the webserver!!
