# PROJECT 11: ANSIBLE - AUTOMATE PROJECT 7 TO 10

- This Project will make you appreciate DevOps tools even more by making most of the routine tasks automated with Ansible Configuration Management, at the same time you will become confident at writing code using declarative language such as YAML. The

  ![design](images/project-11/archi.png)

> ## INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

- Update the name tag on the Jenkins EC2 instance to Jenkins ansible

- In your GitHub account create a new repository and name it ansible-config-mgt.

- Install Ansible - this will be done on an Ubuntu 20.04 instance

  ```
  sudo apt update
  sudo apt install ansible
  ```

- Check the version of the ansible by running:

  `ansible --version`

- Configure Jenkins build job to save the repository content every time a change is done – this will solidify the Jenkins configuration skills acquired in Project 9.
