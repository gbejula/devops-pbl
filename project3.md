# Project 3 - MERN STACK IMPLEMENTATION

> Step 1: Backend Configuration

- Update Ubuntu

  `sudo apt update`

- Upgrade Ubuntu

  `sudo apt upgrade`

- Get the location of Nodejs software from Ubuntu repository

  `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

- Install Node.js

  `sudo apt-get install -y nodejs`

- Verify the Node Installation

  `node -v`

- Verify the npm installation

  `npm -v`

## Set main directory for project

- Create a new directory for the To-Do project
  `mkdir Todo`

- Change directory to the Todo folder and initialize the project

```
cd Todo
npm init
```

> Install Expressjs

`npm install express`

- Create a file, install dotenv and open file in vim

  ```
  touch index.js
  ls - to check the file
  npm install dotenv
  vim index.js
  ```

- Add code to file index and run using:

  `node index.js`
  ![Image](images/project-3/server-running.png)

- Check server in the browser by add port 5000 to public ip

  ![Image](images/project-3/server-running-from-browser.png)

- Create the routes

  ```
  The routes will:
  create a new task
  display the list of all tasks
  delete a completed task
  ```

- Create directory for the route, change to directory and create a file. The routes folder will be created in main Todo foler

  ```
  mkdir routes
  cd routes
  touch api.js
  ```
