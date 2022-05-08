# PROJECT 11: ANSIBLE CONFIGURATION MANAGEMENT- AUTOMATE PROJECT 7 TO 10

- This Project will make you appreciate DevOps tools even more by making most of the routine tasks automated with Ansible Configuration Management, at the same time you will become confident at writing code using declarative language such as YAML. The

  ![design](images/project-11/archi.png)

> ## INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

- Update the name tag on the Jenkins EC2 instance to Jenkins ansible

- In your GitHub account create a new repository and name it ansible-config-mgt.

  ![new repo](images/project-11/new-repo.png)

- Install Ansible - this will be done on an Ubuntu 20.04 instance

  ```
  sudo apt update
  sudo apt install ansible
  ```

- Check the version of the ansible by running:

  `ansible --version`

  ![ansible version](images/project-11/ansible-version.png)

- Configure Jenkins build job to save the repository content every time a change is done – this will solidify the Jenkins configuration skills acquired in Project 9.

  - In the new repo, click the settings tab
  - Select Webhooks
  - Click 'Add Webhooks'
  - In the payload URL, add the Jenkins URL: http://x.x.x.x:8080/github-webhook
  - In the content type, select 'application-json'
  - Click 'Add'

  ![webhook image with ip]

- Configure the freestyle Jenkins build job with name - jenkins-in-ansible

  _Go to Source code management of the jenkin-in-ansible_

  _Select Git_

  _copy the new repo url 'https' and paste in the repository URL_

  ![git url](images/project-11/step1.png)

  _Under Post-build Actions, select 'Archive the artifacts'_

  _In the file to archive type, enter "\*\*"_

  ![step 2](images/project-11/step2.png)

  _Click Save_

- Go back to the Jenkins-in-Ansible project and click on 'Build now"

  _The build failed_

  _I discovered that the needed to change the branch to *main* instead of *master* since it has been changed by github_

  _The build was now successfully after the change_

  ![freestyle build](images/project-11/freestyle-build.png)

- For the automatic build, changed the README.md file

- The build did not automatic trigger. I changed the files in the README.md four times but it still did not automatically trigger.

- Looked and discovered that the "GitHub hook trigger for GITScm polling" was not checked. Then, I checked the option.
