## Elite Corporation Task Management System 
Elite Corporation needs a lightweight Task Management API to help employees manage tasks efficiently. This API should run inside a Docker container and operate without a database, meaning data will reset when the container restarts.

### What is Expected
1. Build a Simple API

Develop a Node.js Express API that allows users to add, view, update, and delete tasks.

Tasks will be stored temporarily in memory (not in a database).

2. Containerize the Application

Create a Dockerfile to package the application into a Docker container.

Ensure the container includes all necessary dependencies and exposes the correct port for access.

3. Test the API

Verify that the API is working correctly by sending requests to add, view, update, and delete tasks.

Testing can be done using Postman or a command-line tool like cURL.

4.  Deploy and Run in Docker

Build the Docker image and run the container.

Ensure the API is accessible inside the containerized environment.

I launched an EC2 instance on the AWS cloud service to SSH into a Linux Ubuntu machine.

![pic](<img/Screenshot (995).png>)

### I Install and start docker

For **Ubuntu**:

1. Update your package index:
   ```bash
   sudo apt update
   ```

2. Install Docker:
   ```bash
   sudo apt install docker.io -y
   ```

3. Start Docker:

   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```
4. Check the Status of Docker
   ```bash
   sudo systemctl status docker
   ```

5. ctrl + c to exit the running docker

![pic](<img>)

I Created a Directory and cd into the directory using the command below

```bash
   mkdir elite-task-api && cd elite-task-api
   ```

![pic](<img/Screenshot (966).png>)


### Set Up the Node.js Express API
   I Initialize a Node.js Project
 ```bash
   sudo apt install npm
   npm init -y
   ```
 I Install required dependencies for express  
 ```bash
   npm install express
   ```

![pic](<img/Screenshot (967).png>)

![pic](<img/Screenshot (968).png>)


  - Created a file app.js by running 

     **vi app.js**

- Input my Configuration.

       // app.js

       const express = require('express');
       const app = express();
       app.use(express.json()); // This is to parse JSON bodies

       let tasks = [
        { id: 1, title: "Fix server issue", status: "Pending" },
        { id: 2, title: "Deploy new feature", status: "Completed" }
       ];

       // POST /tasks - Add a new task
       app.post('/tasks', (req, res) => {
       const { title } = req.body;
       const id = tasks.length + 1; // Generate new ID
       const newTask = { id, title, status: "Pending" };
       tasks.push(newTask);
       res.status(201).json(newTask);
       });

       // GET /tasks - Get all tasks
       app.get('/tasks', (req, res) => {
        res.json(tasks);
        });

       // PUT /tasks/:id - Update a task
       app.put('/tasks/:id', (req, res) => {
       const { id } = req.params;
       const { title, status } = req.body;
       const task = tasks.find(t => t.id === parseInt(id));

        if (!task) return res.status(404).send('Task not found');
  
        task.title = title || task.title;
        task.status = status || task.status;
  
          res.json(task);
        });

       // DELETE /tasks/:id - Delete a task
       app.delete('/tasks/:id', (req, res) => {
       const { id } = req.params;
       const taskIndex = tasks.findIndex(t => t.id === parseInt(id));

        if (taskIndex === -1) return res.status(404).send('Task not found');

        tasks.splice(taskIndex, 1);
        res.status(204).send();
        });

        // Set up the server
        const port = 3000;
        app.listen(port, () => {
        console.log(`Server running on http://localhost:${port}`);
        });

![pic](<img>)
![pic](<img/Screenshot (970).png>)


   - Created a **Dockerfile** by using this command below to create the file

     **touch Dockerfile** 
 

 Edited the Dockerfile using
 vi Dockerfile

   ![pic](<img/Screenshot (975).png>)

- Input the needed configuration

       FROM node:18-alpine
       # Uses the Node.js 18 Alpine Linux base image (lightweight).

       WORKDIR /app
       # Sets the working directory inside the container.

       COPY package*.json ./
       # Copies package.json and package-lock.json to the container.
       # This is done before copying the rest of the application code to leverage Docker's caching.

       RUN npm install
       # Installs Node.js dependencies.

       COPY . .
       # Copies the rest of the project files.

       EXPOSE 3000
       # Exposes port 3000.

       CMD ["node", "app.js"]
       # Runs the Node.js application.

 ![pic](<img/Screenshot (974).png>)

- Created a docker ignore file to stop docker from copying unnecessary files using

     **vi dockerignore**

- input the following to the file

     node_modules
     npm-debug.log

 ![pic](<img/Screenshot (977).png>)

 ![pic](<img/Screenshot (978).png>)


# Build and Run the Docker Container
 Build the Docker Image
 sudo docker build -t elite-task-api .

 ![pic](<img/Screenshot (996).png>)


 solve the error with the following command to run the docker container
  
     **sudo usermod -aG docker ubuntu**


- Then used the following command for changes to take effect
  
     **newgrp docker** 

- I Ran the Docker image and made my containerized docker accessible via port 3000 on my system using

     **docker run -p 3000:3000 elite-task-api**

![pic](<img/Screenshot (998).png>)

After making port 3000 accessible, I added an inbound rule to my EC2 instance to allow access to port 3000

![pic](<img/Screenshot (980).png>)

- Docker image is running live on **port 3000** of my instance
  
 ##### Testing

- To test the **Elite Corporation** api using **postman** I setup mmy postman for the various test as follows

- **GET**
  
-  Created a new http task where I input my ip address, port and set content type to json in the body 
  
     content-type  json
     (http://54.209.38.55:3000/tasks)

- Sent get task in dropdown menu and got the tasks available
  
![pic](<img/Screenshot (982).png>)

- **POST**
   
- Created a new http task where I input my ip address, port and set content type to json in the body 
  
     content-type  json
     (http://54.209.38.55:3000/tasks)

- POST task set in the dropdown menu

![pic](<img/Screenshot (983).png>)

- Body where I input task to be added and selected json
  
![pic](<img>)

- Result as task was added successfully

![pic](<img/Screenshot (984).png>)



- **PUT**
  
-  Created a new http task where I input my ip address, port, task Id and set content type to json in the body 
  
     content-type  json
     (http://54.209.38.55:3000/tasks/2) 
  
- PUT task in the dropdown menu and the task was modified to in progress
  
![pic](<img/Screenshot (986).png>)

- **DELETE**

-  Created a new http task where I input my ip address, port,task ID and set content type to json in the body  
  
     content-type  json
     (http://54.209.38.55:3000/tasks/3)
  
![pic](<img/Screenshot (977).png>)

- Deleted and verified by sending a Get request and just two task were available in the array and new added task was deleted

![pic](<img/Screenshot (988).png>)