# PROJECT 9: CONTINUE INTEGRATION PIPELINE FOR TOOLING WEBSITE

- In this project we will automate the part of our routine tasks with a free and open source automation server - Jenkins. It is one of the most popular CI/CD tools, it was created by a former Sun Microsystems developer Kohsuke Kawaguchi and the project originally had a named "Hudson".

- Acording to Circle CI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy.

> ## TASK

- We are enchancing architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS Server.

  ![Image](images/project-9/Diagram-for-project9.png)

> ## INSTALL AND CONFIGURE JENKINS SERVER

- Create an AWS EC2 server based on Ubuntu server 20.04 LTS and name it "Jenkins" with TCP port 8080 opened in the inbound rule

- Install JDK

  ```
  sudo apt update
  sudo apt install default-jdk-headless
  ```

- Install Jenkins

  ```
  wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
  sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  sudo apt update
  sudo apt-get install jenkins
  ```

- Verify Jenkins is up and running

  `sudo systemctl status jenkins`

  ![Jenkins status](images/project-9/jenkins-running-status.png)

- Perform initial Jenkins setup from the browswer by accessing:

  - Note: open port 8080 by creating a TCP inbound rule in the security group

  `http://jenkins-server-public-ip-address:8080`

  ![Jenkins-in-the-browswer](images/project-9/Jenkins-getting-started.png)

- Retrieve the administrator password from the server using:

  `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

  ![Jenkins-password](images/project-9/jenkins-initial-psd.png)

- Enter the password in the admin page and click continue

  ![Jenkins Plugins](images/project-9/jenkins-install-plugins.png)

- Click Install suggested plugins

  ![Plugins](images/project-9/tools-installed.png)

- Create admin account after plugins installation is complete.

  ![Create admin account](images/project-9/create-an-admin-account.png)

- Click on save and continue

  ![Save configuration](images/project-9/jenkins-last-config-page.png)

- Jenkins is full ready for deployment

  ![Jenkins Ready](images/project-9/jenkins-is-ready.png)

> ## CONFIGURE JENKINS TO RETRIEVE SOURCE CODE FROM GITHUB USING WEBHOOKS

- We will configure a simple Jenkins job/project that will be triggered by GitHub webhooks and will execute a build task to retrieve codes from GitHub and store it locally on Jenkins server.

- Enable webhooks in GitHub repository settings for the tooling forked repository in project 7

  ![WebHooks](images/project-9/jenkins-webhooks.png)
