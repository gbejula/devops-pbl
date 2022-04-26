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
  sudo apt-get update
  sudo apt-get install jenkins
  ```

- Verify Jenkins is up and running

  `sudo systemctl status jenkins`
