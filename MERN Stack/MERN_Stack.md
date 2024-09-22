# MERN Web Stack
In this project, we are tasked with implementing a web solution based on the **MERN stack** in AWS Cloud, focusing on utilizing our skills in **MongoDB, Express.js, React, and Node.js** to deliver a scalable and dynamic application.
MERN stands for:

 *MERN* Web stack consists of following components:
  * MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
  * ExpressJS: A server side Web Application framework for Node.js.
  * ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI)components.
  * Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

    ![image](https://github.com/user-attachments/assets/71db1d60-9178-4ad3-adf7-3257841c6b55)

**Step 0 - Preparing prerequisites**
* Launch Ubuntu Server on EC2
* & Tag a name --> MERN-STACK
* 
![image](https://github.com/user-attachments/assets/07c554ad-3372-48f0-b9ff-46cea48afab5)

![image](https://github.com/user-attachments/assets/326928ac-b02d-4bc5-a9f2-96fd03670d7c)

* connect instance via ssh
  ![image](https://github.com/user-attachments/assets/f40ea107-8e78-4f64-b97c-b90a4958c4ad)



**Step 1 - Backend configuration**

* Update Ubuntu
```bash
sudo apt update -y
```
![image](https://github.com/user-attachments/assets/11c080e0-a132-4069-969e-8c868430181f)


* upgrade ubuntu
```bash
sudo apt upgrade -y
```
![image](https://github.com/user-attachments/assets/df8621db-9ad3-4492-818f-d102a6302b3f)



* Setting Up Node.js 18.x Repository on Debian/Ubuntu Systems

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![image](https://github.com/user-attachments/assets/1a0f4190-1d6a-4332-85d7-6c7db2bffbc6)


* Install Node.js on the server
Install Node.js with the command below

```bash
sudo apt-get install -y nodejs
```
![image](https://github.com/user-attachments/assets/c4cf3bbf-a3a6-4898-a21a-347e6bb272ac)

* verify the node installation
```
node -v
```
![image](https://github.com/user-attachments/assets/ac088abc-3279-45ae-876d-bba4f9a7d30a)


**Application Code Setup**
* create a new directory for your  **TO-DO** Project
  ```
  mkdir todo
  ```
* To check file
    ```
    ls
    ```
![image](https://github.com/user-attachments/assets/87d72ccb-3542-447e-ade0-75a68436efcc)

* change directory
  ```
  cd todo
  ```
* run the command npm init ti inislise your project
  ```
  npm init
  ```
![image](https://github.com/user-attachments/assets/3a29460a-d0be-46c2-bba6-43deb6884e3b)

* confirm  taht
  ```
  ls
  ```
![image](https://github.com/user-attachments/assets/d3dd79cc-59d2-41f7-8e62-cc5cbb93c472)

* **NOTE** Next, we will Install ExpressJs and create the Routes directory.

**Install ExpressJS**
* Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

* To use express, install it using npm:
```
npm install express
```
![image](https://github.com/user-attachments/assets/98dd8b59-2611-4d92-a87c-adb7aeeed8b4)

* Create a file index.js
```
touch index.js
```
![image](https://github.com/user-attachments/assets/8a029347-b0d7-4d03-aa5c-4b9f7aaba4c3)


* install the **dotenv**

```
npm install dotenv
```
![image](https://github.com/user-attachments/assets/5f51d881-ff17-44c6-ae51-f114a904b1a7)

* Open the index.js file with vim
  ```
  vim index.js
  ```
* past the code
  
```
  const express = require('express');
require('dotenv').config();
const app = express();
const port = process.env.PORT || 5000;

app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    next();
});

app.use((req, res, next) => {
    res.send('Welcome to Express');
});

app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});

```
![image](https://github.com/user-attachments/assets/c0a6c879-3cad-4eb3-8964-f199cbf225d2)

Now it is time to start our server to see if it works. Open our terminal in the same directory as our index.js file and type:

```
node index.js
```

![image](https://github.com/user-attachments/assets/30b7bb17-dd1b-4883-93e9-a620658797ba)


Now we need to open 5000 port in EC2 Security Groups:

![image](https://github.com/user-attachments/assets/0a74108a-e272-4805-93d1-a1354c2834f9)


* open browser
  ```
  http://public-ip:5000
  ```

![image](https://github.com/user-attachments/assets/70a3a49b-8994-452c-8a14-581377d0f60a)


**Routes**
There are three actions that our To-Do application needs to be able to do:
* Create a new task
* Display list of all tasks
* Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes
```
mkdir routes
```

```
cd toutes
```
* create a file api.js
```
touch api.js
```
* open the file
```
vim api.js
```

* copy & paste
  
```
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {});

router.post('/todos', (req, res, next) => {});

router.delete('/todos/:id', (req, res, next) => {});

module.exports = router;
```

![image](https://github.com/user-attachments/assets/691626d8-79ce-4c77-acf4-f1f2a635070f)

* Moving forward let create Models directory.
  
**Models**

**Introduction to Models in JavaScript Applications**
When building JavaScript-based applications, especially those using MongoDB (a NoSQL database), a model is essential. It defines how your data is structured and helps interact with the database in an organized way.

**Key Concepts:**
**MongoDB and NoSQL Database**

MongoDB is a NoSQL database that stores data in a flexible, JSON-like format called documents.
Unlike traditional SQL databases, NoSQL databases like MongoDB don’t rely on fixed tables or schemas, making them more flexible.

**What is a Model?**

In JavaScript-based applications, especially with MongoDB, a model is a key component.
Models define how data is structured, how it interacts with the database, and help make the app interactive.

**The Role of Schema in Models**

A Schema is essentially the blueprint for your MongoDB documents.
It defines the fields (properties) and data types that each document will have in the database.
Think of the schema as a map for how your data is organized.

**Virtual Properties**

In a Schema, some properties may not be stored directly in the database. These are called virtual properties.
They are useful for data manipulation or computing values on the fly without saving them directly in the database.

* To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.
```
npm install mongoose
```


![image](https://github.com/user-attachments/assets/cf47b43e-2613-442b-bcf2-06469b8fb621)

* Create a new folder with mkdir models command
```
mkdir models
```
* Change directory into the newly created 'models' folder with cd models.
```
cd models
```

* Inside the models folder, create a file and name it todo.js

```
touch todo.js
```

![image](https://github.com/user-attachments/assets/fbc8415b-919e-4d82-a157-09aa5d230b69)

* copy & past

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// create schema for todo
const TodoSchema = new Schema({
    action: {
        type: String,
        required: [true, 'The todo text field is required']
    }
});

// create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```


![image](https://github.com/user-attachments/assets/027f6f05-a55a-4c37-9b1e-092b33322c1b)

