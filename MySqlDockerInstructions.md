**Heads Up
This is a guide that helped me get MySql server up and running **MUCH** faster than the VMs. It’s especially nice if you’re off campus/having issues with the VMs. It allows you do do things locally with the power of your computer instead of the VM. 

**What it do**
I ran this on Fedora 29 and have used a similar thing on the most recent OSX Mojave (I don’t wanna go check the exact version)


If you don’t have docker downloaded and installed go ahead and do that

Installation instructions for your OS from the documentation (they actually have good docs)
https://docs.docker.com/install/ 

I used this for most of it:
https://www.serverlab.ca/tutorials/containers/docker/how-to-run-mysql-server-8-in-a-docker-container/

Pull the docker image you want to use:
```
docker pull mysql/mysql-server
```

This will create the directory  where you want to store the database information and such
```
mkdir -p /data/mysql
```

Run the docker image
```
docker run -d -p 3306:3306 -v /data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mypassword mysql/mysql-server 
```


For mypassword obviously you want to put something different
>Yes, I am well aware this horrible practice. Instead you should save the password as an environment variable and pass that in. When I tried that I had issues hence why it is not included. 

>## What this does:
> `run` tells docker to run the image you specify later
> `-d` tells docker to run the container to run in detached mode so you can still use your terminal
> `-p` tell docker to map your port 33306 to the machine port 3306 (default mysql port)
>> Ex of a different port mapping would be `-p 1337:3306` which would map mysql container port 3306 to port 1337 of your machine 
> `-v` maps a volume (directory) on your drive to a volume on the machine `<your volume>` : `<docker machine’s volume>`
> `-e` sets environment variables of your machine, in this case the mysql root password

*Note: I did not set a password since I already have one

Let’s see if the container is running :
```
docker container ls
```

Now that the container is running you want to be able to use it:
```
docker exec -it <container name> bash
```

Bam! Drops you into a root shell 

*Note: to exit bash type `exit`
Now to use mysql:
```
mysql -uroot -p
```
Or
```
mysql -p
```

It will prompt you for a password. That will be the password you supplied in the first part. 
In the mysql prompt you now can do your thing
*Note to exit mysql type `quit`

To show databases:
```
show databases;
```
To create databases:
```
CREATE DATABASE databaseName;
```
To create a user:
```
CREATE USER '<username>'@'localhost' IDENTIFIED BY '<password>';
```
Also according this the default password hashing mechanism in 8+ is sha2 which meets the encrypted format specs for *this section*

https://dev.mysql.com/doc/mysql-security-excerpt/8.0/en/caching-sha2-pluggable-authentication.html 

Give the user privileges:
```
GRANT ALL PRIVILEGES ON databaseName.* TO ‘<user>’@'localhost';
FLUSH PRIVILEGES;
QUIT
```
Show the users and what hosts they can connect from:
```
SELECT host, user FROM mysql.user;
```
It will display a table, for example like this:
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

It has to contain a line with your database user and '%' to works (% means "every IP addresses are allowed"). Example:
+------------+------------------+
| host       | user             |
+------------+------------------+
| %          | root             |
+------------+------------------+

https://stackoverflow.com/questions/16287559/mysql-adding-user-for-remote-access

How to create a user to use to connect to your database:
```
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypass';
CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypass';
```
Now give them some privileges:
```
GRANT ALL ON *.* TO 'myuser'@'localhost';
GRANT ALL ON *.* TO 'myuser'@'%';

```

Then just a connector like you normally would. In this case we’re running the server locally (easiest way). You can also connect remotely, but that involves port forwarding and setting up dynmaic dns or a static IP which I won’t get into.


In my case I am running it in DataGrip, but you can use MySql Workbench or whatever floats your boat:

