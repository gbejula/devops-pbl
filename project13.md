# PROJECT 13: ANSIBLE DYNAMIC ASSIGNMENTS (INCLUDE) AND COMMUNITY ROLES.

> ## STEP 1 - INTRODUCING DYNAMIC ASSIGNMENT INTO OUR STRUCTURE

- In your https://github.com/gbejula/ansible-config-mgt GitHub repository start a new branch and call it dynamic-assignments.

  ![new branch](images/project-13/new-branch.png)

- Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml.

  ![dynamic image](images/project-13/file-structure.png)

> ## STEP 2 - UPDATE SITE.YML WITH DYNAMIC ASSIGNMENTS

- Update site.yml file to make use of the dynamic assignment. site.yml should now look like this.

```
---
- hosts: all
- name: Include dynamic variables
  tasks:
  import_playbook: ../static-assignments/common.yml
  include: ../dynamic-assignments/env-vars.yml
  tags:
    - always

-  hosts: webservers
- name: Webserver assignment
  import_playbook: ../static-assignments/webservers.yml
```

- Download Mysql Ansible Role
