# PROJECT 15: AWS SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY

## Step 1: Setup a Virtual Private Cloud

- Create a VPC using a large enough CIDR block (/16) & enable DNS hostnames

- Create subnets as shown in the diagram above. For the public subnet, we create 2 subnets in availability zone A and B respectively and for the private subnet we create 4 subnets.

* Create a route table and associate it with the public subnets
  - Select the route table you created, click Actions on the top and click 'Edit Subnet associations'
  - Select the public subnets and click save
* Create a route table for the private subnets
  - Repeat the steps above
* Create an Internet Gateway, select it and click Actions, then click Attach to VPC and attach it to the VPC you created
* Add a new route to your public subnet route table

  - Select the route table, click Actions and 'Edit routes'
  - For destination, enter 0.0.0.0/0
  - For target, select Internet Gateway and click the Internet Gateway you created
    Click Save

* Create an Elastic IP address that will be used by the NAT-Gateway
* Create a NAT Gateway for your private subnets (create one in each public subnet)and attach the Elastic IP created

* Add a new route to your private route table with destination as 0.0.0.0/0 and target as the NAT Gateway you created
* Associcate route table with subnets

* Create a security group for:
  - Application Load Balancer: ALB should be open to the internet
  - Bastion servers: Access to bastion servers should only be from the IPs of your workstation
  - Nginx servers: Access to nginx servers should only be from the Application Load Balancer and bastion
  - Internal lb: Allow access to nginx servers
  - Webservers: Webservers should only be accessible from the Nginx servers and bastion
  - Data Layer: This comprises the RDS and EFS servers. Traffic from Bastion (Mysql), Webserver (NFS and Mysql).

## Step 2: Proceed with Compute Resources

Create Autoscaling group : The two requirements are Launch Templates and Load Balancers. The Launch Templates requires AMI and Userdata while the Load balancer requires target goup

The following steps below must take place before creating the Autoscaling group

# Step 2a: Setup Compute Resources for Nginx

- Provision an EC2 Instances for Nginx, bastion and webserver
  - Create a t2.micro RHEL 8 instance in any of your two public AZs
  - Install the following packages

```
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
yum install wget vim python3 telnet htop git mysql net-tools chrony -y
systemctl start chronyd
systemctl enable chronyd
```

- Disable senten so that our servers can function properly on all the redhat instance (nginx & webserver only)

```
setsebool -P httpd_can_network_connect=1
setsebool -P httpd_can_network_connect_db=1
setsebool -P httpd_execmem=1
setsebool -P httpd_use_nfs 1
```

- Install Amazon efs utils for mounting targets on the elastic file system (Nginx & webserver only)

```
git clone https://github.com/aws/efs-utils
cd efs-utils
yum install -y make
yum install -y rpm-build
make rpm
yum install -y  ./build/amazon-efs-utils*rpm
```

- Install self signed certificate on nginx

* _The reason we are installing this is because the load balancer will be sending traffic to the webserver via port 443 and also listen on port 443 thus for the connection to be secured we need a self signed certificate on the nginx instance_

- _Make sure you enter the private IPv4 dns of the instance_

```
sudo mkdir /etc/ssl/private
sudo chmod 700 /etc/ssl/private
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ACS.key -out /etc/ssl/certs/ACS.crt
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

- confirm if the ACS.crt and ACS.key file exist
  ![pic9](./images/pic9.png)

- Install self signed certificate for the webserver

```
yum install -y mod_ssl
openssl req -newkey rsa:2048 -nodes -keyout /etc/pki/tls/private/ACS.key -x509 -days 365 -out /etc/pki/tls/certs/ACS.crt
vi /etc/httpd/conf.d/ssl.conf
```
