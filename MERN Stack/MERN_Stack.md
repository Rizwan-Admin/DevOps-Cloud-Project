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


* Now we need to update our routes from the file api.js in 'routes' directory to make use of the new model.
*  In Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit

```
vim api.js
```
copy and past
```
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

// Get all todos (only exposing the id and action field)
router.get('/todos', (req, res, next) => {
  Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next); // Pass the error to the next middleware
});

// Create a new todo
router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next); // Pass the error to the next middleware
  } else {
    // Respond with status 400 for bad request
    res.status(400).json({ error: "The input field is empty" });
  }
});

// Delete a todo by id
router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({ "_id": req.params.id })
    .then(data => {
      if (data) {
        res.json(data); // If todo is found and deleted
      } else {
        // If the id does not match any todo, respond with 404
        res.status(404).json({ error: "Todo not found" });
      }
    })
    .catch(next); // Pass the error to the next middleware
});

module.exports = router;
```
![image](https://github.com/user-attachments/assets/e12f0b81-76b4-4fdf-90f2-6cf94edcb994)

* The next piece of our application will be the MongoDB Database

  **MongoDB Database**
  We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. Sign up here. Follow the sign up process, select AWS as the cloud provider, and choose a region near you.

  ![image](https://github.com/user-attachments/assets/79064502-405b-4e11-8349-78c984ffcfd9)
  
1. Create a MongoDB database and collection inside mLab

MongoDB Cluster Overview
![image](https://github.com/user-attachments/assets/da6ed31e-33e6-423c-81c3-76ca68907837)


* Network Access
![image](https://github.com/user-attachments/assets/e50d8224-0cef-4c97-add9-5d14fafc228a)

create Databaase
![image](https://github.com/user-attachments/assets/39c55ab1-a5d8-40c9-8e6f-ad2486823e58)


* 2. Create a file in your Todo directory and name it .env, open the file
 
 ```
touch .env && vim .env
```

* Add the connection string to access the database in it, just as below:
```
DB = ‘mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority’
```

![image](https://github.com/user-attachments/assets/812970ff-4646-473d-8ecc-ec0998b99c55)


3. Update the index.js to reflect the use of .env so that Node.js can connect to the database.
```
vim index.js
```

copy & past

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

// Connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log(`Database connected successfully`))
  .catch(err => console.log(err));

// Since mongoose promise is deprecated, we override it with Node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
  console.log(err);
  next();
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
Start your server using the command:

```
node index.js
```



  

