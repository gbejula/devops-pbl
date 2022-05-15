# PROJECT 12: ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

- Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

> ## STEP 1 - JENKINS JOB ENHANCEMENT

- Go to the Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

  `sudo mkdir /home/ubuntu/ansible-config-artifact`

  ![directory created](images/project-12/artifact-directory.png)

- Change permissions to this directory, so Jenkins could save files there –

  `chmod -R 0777 /home/ubuntu/ansible-config-artifact`

- Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

  ![success](images/project-12/install-copy-artifacts.png)

  ![copy plugin installed](images/project-12/plugin-installed.png)

- A new freestyle project created called **save_artifacts**

- The project will be triggered by completion of your existing ansible project. Configure it accordingly

  ![first step](images/project-12/step1.png)

  ![second step](images/project-12/step2.png)

- The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

  ![third step](images/project-12/step3.png)

- Test the set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).

  ![trigger success](images/project-12/artifact-project-successful.png)
