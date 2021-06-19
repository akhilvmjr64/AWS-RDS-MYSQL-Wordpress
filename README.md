In this article I am going to show how to create a database in Relational Database Service(RDS) of AWS, how to install WordPress server on an EC2-instance, and then how to connect the Database to the WordPress server.

### Creating a database in AWS RDS:
Firstly to create the Database we need to go to the RDS dashboard of AWS and then click on create database. Then we need to give the configuration for the RDS. Following is the configuration that I gave while launching the database:
- ![task18a.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624121981458/P2Tlu9AIr.png)
    Selected the database creation method as default
- ![task18b.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624122039844/LD7Sk3Psl.png)
    Selected the database engine as MySQL. WordPress only supports certain versions of MySQL hence choose the 5.7.31 which works with the WordPress.
- ![task18c.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624122165260/t5Lt9ydr1.png)
    Then I have chosen the free tire template
- ![task18d.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624122337628/yjw8FCDA_.png)
    Then entered the database identifier. AWS provides the endpoint with this database identifier. Then I have given the username and the password for the database. This data will be used for authentication.
- ![task18e.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624122504628/YnJYA53UY.png)
    Free tire by default has db.t2.micro as the option selected for the database instance type. I have left that unchanged
- ![task18f.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624122605249/QmAEPB0fv.png)
    Then I have allocated storage of 10GB to the database. Also there is option for autoscaling which is not necessary for my usecase but left it as default as I am just creating a simple server
- ![task18g.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624122660814/68flB42Sz.png)
    Then I have left the VPC as default as I will be launching the instance in the default VPC. Then I choose an already existing security group which allows all the traffic.
- ![task18h.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624122866376/pR12Dum-SA.png)
    Keeping the remaining options as is I started creating the database. It took around 5-10 mins to launch the database

### Configuring the WordPress server:

In this 5-10mins gap I launched an instance using RedHat AMI. Then I have connected to the instance using ssh from my local machine. And then logged into the root shell by using the command `sudo su`.

- ![task18i.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624123209259/JOsL3ieOqk.png)
    Then I have ran the `yum update -y` command which is a better practice. Above is the output I got after doing it
- ![task18j.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624123313723/wmmgSSdt1.png)
    Then I have installed the installed the packages `php`, `php-mysqlnd`, `php-fpm`, `httpd`, and `php-json` using the command `yum install php php-mysqlnd php-fpm httpd php-json -y`
- ![task18k.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624123729822/f-ddXxxoe.png)
    Then I have dowloaded the wordpress server code using the command `curl -O https://wordpress.ord/latest.tar.gz`.
    ![task18l.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624123882728/L0RHytBYT.png)
    Then I have extracted the tarfile using the command `tar xfz latest.tar.gz`. Then I have copied the WordPress content to the `/var/www/html/` which is the default root document for the HTTPD server.
- ![task18w.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624124251555/JtYdYOEAA.png)
    Then I have set the SELinux to permissive mode using the command `setenforce 0` which will disable the SELinux. It will cause some issues during the installation hence disabled it.
- ![task18x.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624124360015/jx7198NKla.png)
    Then I have started and enabled the htttpd service

### Configuring WordPress webapplication:
---
After the WordPress server is installed properly I opened the webapplication which shows the homepage as follows

- ![task18n.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624124546792/fBcRrKxTT3.png)
    If this screen is displayed then it is an acknowledgement that wordpress server is installed properly. Clicking on let's go will take to the next page.
- ![task18o.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624124652759/ft274mC0-k.png)
    This page is where we need to give the details about the database server. If we see this page it is asking for a database.
    ![task18p.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624124815558/60sCYlYHY.png)
    As the WordPress server needs a database that is already created in it hence I have installed the mysql client to access the database server from the WordPress server using the command `yum install mysql -y`.

![task18q.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624125207421/tE-b3su_t.png)
    After installing the mysql client I have used the command `mysql -h <host_name> -u <username> -p<password>` to connect to the database. Here &lt;host_name&gt; is endpoint of the database server, &lt;username&gt; is the name of the user created, &lt;password&gt; is the password for the user. There should be no space after `-p` option while giving the password. Then I have created the database named wordpress using the query `create database wordpress;`

- ![task18o.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624125308327/qNZ55c8tX.png)
    Now coming back to where I left on the webpage I submitted it after the database is created.

- ![task18r.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624125397790/K55c1BI63.png)
   Then it returned a screen as above. Clicking on run the installtion will take to another page as following
   ![task18s.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624125534122/TVzN-quG6.png)
    On this page I gave the details and submited the form

    ![task18t.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624125571834/UhyhVXgqT.png)
    Then it gave a success screen as above

    ![task18u.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624125609013/bgpOs6KgCr.png)
    Now we need to provide the login details and login to the wordpress server. Which displayed the screen as follows which implies that wordpress is installed correctly and the database is connected to it.
    ![task18v.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624125761787/95dZZe6B3Q.png)

This is how I launched the database in RDS and used it for the WordPress server running on the EC2 instance.
