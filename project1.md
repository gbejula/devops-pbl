# PROJECT 1: LAMP STACK IMPLEMENTATION

> WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

- Create AWS account and set up ubuntu server

- Connected to the EC2 instance.

- Navigated to download to copy for the **.pem** file

- Ran the following command

  ```
  sudo chmod 0400 <private-key-name>.pem
  ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
  ```

  ![chmod](images/project-1/Screenshot%20from%202022-04-06%2015-13-51.png)

> ## Step 1 — Installing Apache and Updating the Firewall

- Install apache on ubuntu instance using apt:

  ```
  sudo apt update
  sudo apt install apache2
  ```

- Check status of apache

  `sudo systemctl status apache2`

  ![status of apache](images/project-1/Screenshot%20from%202022-04-06%2015-18-16.png)

- Check from console

  ![](images/project-1/Screenshot%20from%202022-04-06%2015-19-40.png)

- Check on the browser

  ![browser check](images/project-1/Screenshot%20from%202022-04-06%2015-21-07.png)

> ## Step 2 — Installing MySQL

- To install mysql

  ```
  sudo apt install mysql-server
  sudo mysql_secure_installation
  ```

  ![mysql](images/project-1/Screenshot%20from%202022-04-06%2015-33-10.png)

> ## Step 3 — Installing PHP

- To install these 3 packages at once, run:

  `sudo apt install php libapache2-mod-php php-mysql`

  ![php](images/project-1/Screenshot%20from%202022-04-06%2015-34-05.png)

> ## Step 4 — Creating a Virtual Host for your Website using Apache

- For the virtual host

  ```
  sudo mkdir /var/www/projectlamp
  sudo chown -R $USER:$USER /var/www/projectlamp
  sudo vi /etc/apache2/sites-available/projectlamp.conf
  ```

  ![host](images/project-1/Screenshot%20from%202022-04-06%2015-41-43.png)

> ## Step 5 — Enable PHP on the website

- Enable php on the site

  ```
  sudo vim /etc/apache2/mods-enabled/dir.conf
  sudo systemctl reload apache2
  vim /var/www/projectlamp/index.php
  ```

  ```
  <?php
  phpinfo();
  ```

  ![php in browser](images/project-1/Screenshot%20from%202022-04-06%2015-55-49.png)
