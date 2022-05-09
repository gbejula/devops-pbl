# PROJECT 11: ANSIBLE CONFIGURATION MANAGEMENT- AUTOMATE PROJECT 7 TO 10

- This Project will make you appreciate DevOps tools even more by making most of the routine tasks automated with Ansible Configuration Management, at the same time you will become confident at writing code using declarative language such as YAML. The

  ![design](images/project-11/project-setup.png)

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

  ![webhook image with ip](images/project-11/webhook.png)

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

  _I discovered that the needed to change the branch to **main** instead of **master** since it has been changed by github_

  _The build was now successfully after the change_

  ![freestyle build](images/project-11/freestyle-build.png)

- For the automatic build, changed the README.md file

- The build did not automatic trigger. I changed the files in the README.md four times but it still did not automatically trigger.

- Looked and discovered that the "GitHub hook trigger for GITScm polling" was not checked. Then, I checked the option.

- The changed the README.md file again, and the auto build was triggered.

  ![auto build](images/project-11/auto-build.png)

- Changes in github and checking through the console is the same

  ![git changes](images/project-11/changes-in-github.png)

- Checking the changes in the Jenkins build artifacts using:

  ```
  cd /var/lib/jenkins/jobs/<new of build>/builds/<build_number>/archive/
  cd /var/lib/jenkins/jobs/ansible-in-jenkins/builds/6/archive/
  cat README.md
  ```

  ![console changes](images/project-11/same-changes-in-console.png)

> ## PREPARE YOUR DEVELOPMENT ENVIRONMENT USING VISUAL STUDION CODE

- Download and install VS code

- Install Remote development extension

- Clone the ansible-config-mgt repo

> ## BEGIN ANSIBLE DEVELOPMENT

- In the ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.

- Checkout the newly created feature branch to your local machine and start building your code and directory structure

- Create a directory and name it playbooks – it will be used to store all your playbook files.

- Create a directory and name it inventory – it will be used to keep your hosts organised.

- Within the playbooks folder, create your first playbook, and name it common.yml

- Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

> ## SET UP AN ANSIBLE INVENTORY

- An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

- Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

  ```
  eval `ssh-agent -s`
  ssh-add <path-to-private-key>
  ```

- Confirm the key has been added with the command below, you should see the name of your key. This should be done from the command line

  `ssh-add -l`

- Now, ssh into your Jenkins-Ansible server using ssh-agent.

  `ssh -A ubuntu@public-ip`

  ![eval settings](images/project-11/eval.png)

- Update the inventory/dev.yml file with this snippet of code:

  ```
  [nfs]
  <NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

  [webservers]
  <Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
  <Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

  [db]
  <Database-Private-IP-Address> ansible_ssh_user='ec2-user'

  [lb]
  <Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
  ```

  ![vscode showing code](images/project-11/files-from-github.png)

  ![files in console](images/project-11/files-in-console.png)

> ## CREATE A COMMON PLAYBOOK

- In common.yml playbook, write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

- Update the playbooks/common.yml file with following code:

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt:
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```

> ## UPDATE GIT WITH THE LATEST CODE

- Commit the code into GitHub:

- Use git commands to add, commit and push to the newly created branch on GitHub.

  ```
  git status

  git add <selected files>

  git commit -m "commit message"
  ```

- Create a Pull request (PR)

- Review the code and if everything is fine, merge the code to the main branch.

- Head back to the terminal, checkout from the branch into the main, and pull down the latest changes.

- Jenkins will also trigger a job/build since there are changes in the github files.

  ![Changes in Jenkins](images/project-11/files-synced-jenkins.png)

  ![changes in console](images/project-11/files-in-console.png)

> ## RUN FIRST ANSIBLE TEST

- Following one of the videos from Darey.io, tried to ssh into the jenkins-ansible instance from vscode but got an error.

  ![error](images/project-11/error.png)

- I noticed that the error message was showing "permission denied". I reached out to the community, and I got help that the .pem file was not seen, hence, reason for permission denied.

- Then, ran the following:

  ![eval](images/project-11/eval.png)

  ![success](images/project-11/wireshark-in-all-server.png)

- Check all the servers to confirm that wireshark is installed. Confirmed on all servers that wireshark is installed.

  ![confirm wireshark](images/project-11/confirm-from-individual-server.png)
