# **Load Balancer Solution With Apache**

In this project, we'll be implementing a load balancer solution using the Apache Web service. Before we begin, there has to be an understanding about how load balancers work and the different methods & algorithms for configuring them.

Loadbalancing is the efficient/methodical distribution of network or traffic across multiple servers. Every load balancer sits between client devices & the servers, recieving and distributing incoming requests to any avialable server capable of fulfilling them. 

![Load-Balancer](https://user-images.githubusercontent.com/47898882/128514190-661fcc26-9fff-44f8-9fe1-3d3bb911a140.jpg)


This technique of distributing load/requests across servers aids scalbility, efficiency and reliability among many other benefits. A practical example will be to think about the way facebook as a platform is able to serve billions of users, this is made possible by the concept of loadbalancing which allows them to configure loadbalancers that distributes requests to the millions of servers they have based on the loadbalancing algos.

There are several load balncing Algos such as:
- Round Robin
- Least Connections
- Least Time 
- Hash 
- IP Hash

A load balancer will be configured on this project (Devops Tooling Website). The setup consists of two webservers mounted on an nfs server so as to have a shared storage system, and a database server for storing data. [https://github.com/brpo01/devops-tooling-7](https://github.com/brpo01/devops-tooling-7/blob/master/devops-tooling-7.md).We'll see how to configure apache as a loadbalancer

# **CONFIGURE APACHE AS A LOAD BALANCER**
- Spin an Ubuntu Server EC2 instance and make sure to allow inbound connections from port 80.

- Install apache & libxml2-dev on the instance

```
$ sudo apt update
$ sudo apt install apache2 -y
$ sudo apt-get install libxml2-dev
```
- Enable the following modules for the loadbalancer

```
$ sudo a2enmod rewrite
$ sudo a2enmod proxy
$ sudo a2enmod proxy_balancer
$ sudo a2enmod proxy_http
$ sudo a2enmod headers
$ sudo a2enmod lbmethod_bytraffic
```
- Restart the apache service

```
$ sudo systemctl restart apache2
```

- To configure the load balancer for the two servers from the devops tooling website, update the apache default configuration file and add the private ip addresses of the servers

```
$ sudo vi /etc/apache2/sites-available/000-default.conf
```
![8a](https://user-images.githubusercontent.com/47898882/128518552-17528853-e704-4a32-b69c-27b25dd4e33e.JPG)

From the image above, you can see that the load balancing method `lbmethod` used for configuring this setup is `bytraffic`, which means the distribution of incomung load/requests is based on the currrent traffic load. Other load balancing methods include `bybusyness`, `byrequests`, `heartbeat`.

- Test your setup on the browser using the load balancer public ipaddress

![2](https://user-images.githubusercontent.com/47898882/128518547-7299f42d-2e30-47e8-9be6-55e412e09151.JPG)

Congratulations!!. You have just configured an apache load balancer for your devops tooling website.

