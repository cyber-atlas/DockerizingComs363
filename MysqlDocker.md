MySQL
===
Using Docker helped me get a MySQL server up and running _**much**_ faster than the VMs. 
It’s especially nice if you’re off campus/having issues with them. 

### What it do
I ran this on Fedora 29 and have used it similarly on OS X Mojave.

I used these instructions for most of it: <br>

[The Referenced Guide](https://www.serverlab.ca/tutorials/containers/docker/how-to-run-mysql-server-8-in-a-docker-container/)

### How It Do
Pull the docker image you want to use (mysql-server):
```console
user@computer:~$ docker pull mysql/mysql-server
```

![mysqlDocker1](https://user-images.githubusercontent.com/10457502/57586762-0d8ca080-74ea-11e9-99be-315ac9273b0a.png)

This will create the directory in which to store the database information and the like: 
```console
user@computer:~$ mkdir -p /data/mysql
```
![mysqlDocker2](https://user-images.githubusercontent.com/10457502/57586763-0e253700-74ea-11e9-9721-7edabec97458.png)

Run the docker image:
```console
user@computer:~$ docker run -d -p 3306:3306 -v /data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mypassword mysql/mysql-server 
```

![mysqlDocker3](https://user-images.githubusercontent.com/10457502/57586764-0e253700-74ea-11e9-97a4-32739290a93b.png)

>For mypassword obviously you want to put something different. 
>>Yes, I am well aware this horrible practice. Instead you should save the password as an environment variable and pass that in. When I tried that I had issues hence why it is not included. 

What the above command does:

> **`run`** tells docker to run the image you specify later <br>
> **`-d`** tells docker to run the container to run in detached mode so you can still use your terminal<br>
> **`-p`** tell docker to map your port 33306 to the machine port 3306 (default mysql port)<br>
> > Ex of a different port mapping would be **`-p 1337:3306`** which would map mysql container port 3306 to port 1337 of your machine <br>

> **`-v`** maps a volume (directory) on your drive to a volume on the machine **`<your volume>` : `<docker machine’s volume>`** <br>
> **`-e`** sets environment variables of your machine, in this case the mysql root password <br>

_**Note:** I did not set a password since I already have one._

Let’s check if the container is running :
```console
user@computer:~$ docker container ls
```

![mysqlDocker4](https://user-images.githubusercontent.com/10457502/57586765-0e253700-74ea-11e9-9720-03c418c758f9.png)

Now that the container is running you want to be able to use it:
```console
user@computer:~$ docker exec -it <container name> bash
```

![mysqlDocker5](https://user-images.githubusercontent.com/10457502/57586766-0e253700-74ea-11e9-85c2-62dd36d47ae6.png)

Bam! You are dropped into a root shell.

_**Note:** to exit bash type `exit`_

Now to use MySQL:
```shell
mysql -uroot -p
```
Or

```shell
mysql -p
```

![mysqlDocker6](https://user-images.githubusercontent.com/10457502/57586767-0e253700-74ea-11e9-9c65-2b1fad6f96b4.png)

You will be prompted to enter a password. 
That will be the password supplied in the first part. 
Now you can do what you need to. <br>

_**Note:** to exit mysql type `quit`_

### Some Helpful Hints

To show databases:
```mysql
SHOW DATABASES;
```

To create databases:
```mysql
CREATE DATABASE databaseName;
```

To create users:
```mysql
CREATE USER '<username>'@'localhost' IDENTIFIED BY '<password>';
```

According to the documentation the default password hashing mechanism in versions 8+ is SHA2.
SHA2 meets the encrypted format for users specified in the project spec. <br>

[Documentation](https://dev.mysql.com/doc/mysql-security-excerpt/8.0/en/caching-sha2-pluggable-authentication.html) 

Give a user privileges:
```mysql
GRANT ALL PRIVILEGES ON databaseName.* TO ‘<user>’@'localhost';
FLUSH PRIVILEGES;
QUIT
```

Show the users and which hosts they can connect from:
```mysql
SELECT host, user FROM mysql.user;
```

It will display a table, for example like this:
```
+------------+------------------+
| host       | user             |
+------------+------------------+
| %          | root             |
| 127.0.0.1  | root             |
| ::1        | root             |
| localhost  | mysql.sys        |
| localhost  | root             |
| localhost  | sonar            |
+------------+------------------+
```

It has to contain a line with your database user and '%' to work globally (% means "all IP addresses are allowed"). 
Example:
```
+------------+------------------+
| host       | user             |
+------------+------------------+
| %          | root             |
+------------+------------------+
```
[Related Stack Overflow Article](https://stackoverflow.com/questions/16287559/mysql-adding-user-for-remote-access)

How to create a user to use to connect to your database:
```mysql
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypass';
CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypass';
```

Now give them privileges:
```mysql
GRANT ALL ON *.* TO 'myuser'@'localhost';
GRANT ALL ON *.* TO 'myuser'@'%';
```

Then just set up a connector like you normally would. 
In this case we’re running the server locally (the easiest way). 
You may also connect remotely, but that involves port forwarding and setting up dynamic DNS or a static IP which I won’t get into.

In my case I am running it in DataGrip, but you can use MySql Workbench or whatever floats your boat:

![mysqlDocker7](https://user-images.githubusercontent.com/10457502/57586768-0e253700-74ea-11e9-8ef5-39d87d51273f.png)
