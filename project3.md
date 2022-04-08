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

> **INSTALL EXPRESSJS**

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

  > **CREATE MODELS DIRECTORY IN THE MAIN DIRECTORY AND INSTALL MONGOOS**

  `npm install mongoose`

- Create models folder, change to the folder and create a file

  ```
  mkdir models
  cd models
  touch todo.js

  or

  mkdir models && cd models && touch todo.js
  ```

- Add code in the todo file

> **SET UP MONGODB DATABASE**

- Create mongodb account and database

- Create .env file in the root directory

  ```
  cd Todo
  touch .env
  vi .env

  ```

- Add the connection string to access the database

  Ensure to update <username>, <password>, <network-address> and <database> according to your setup

  `DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`

  ![Db connected](images/project-3/db_connected.png)

- Check routes in API routes using Postman

  `post request`

  ![Post Image](images/project-3/post-request.png)

  `get request`

  ![Get Image](images/project-3/get-request.png)

  `delete request`

  ![Delete Image](images/project-3/delete-request.png)
