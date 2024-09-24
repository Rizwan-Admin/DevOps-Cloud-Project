Content

* ### Backend Configration
* ### Install ExpressJS

* 
# MERN (MongoDB, ExpressJS, ReactJS, NodeJS)

### Step 0 - Prerequisites:
* Ubuntu
* ### Launch an instance:
  ![image](https://github.com/user-attachments/assets/22817b49-d870-4fda-b402-269346c00318)

* ### Instance :
![image](https://github.com/user-attachments/assets/d49d12a1-73e5-4094-a5e2-872e5ea3b846)

* ### connect to instance via ssh client:
  ![image](https://github.com/user-attachments/assets/bd54295d-b364-445f-8a1f-6267fd4cfed6)

### Backend Configration:

Update ubnutu 
```
sudo apt update -y
```
![image](https://github.com/user-attachments/assets/3049aa02-451f-437e-87e4-823189c71575)

upgrade ubuntu
```
sudo apt upgrade -y
```
![image](https://github.com/user-attachments/assets/6ed0794a-fb36-42c1-ba5d-50c623520b96)

* ### Node js softwear for ubuntu
  ## Setting Up Node.js 18.x on Debian-Based Systems

To install Node.js 18.x, run the following command:

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![image](https://github.com/user-attachments/assets/e02da9cb-0429-4223-9c36-0f1234a242ec)


* ### After running the setup script, install Node.js by executing:
```
sudo apt-get install -y nodejs
```
![image](https://github.com/user-attachments/assets/c00fbb54-aae1-4c4a-b867-99bb9c9d6e25)


* ### Once the installation is complete, verify the Node.js ans npm version using:
```
node -v
```
&

```
npm -v
```

![image](https://github.com/user-attachments/assets/b473aee1-3381-4ca0-841a-23aa360d34cb)


### Application Code Setup
* ### Create a new directory for TO-DO project:
```
mkdir Todo
```
To verify by ls command

```
ls
```
* ### change the directory

```
cd Todo
```
![image](https://github.com/user-attachments/assets/2760cac0-eefb-410a-be0a-015be0f776d3)

* Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

![image](https://github.com/user-attachments/assets/2b47f316-a3b9-4637-8d3a-6d48aa3a8241)

  
* verfiy runthe command ls
```
ls
```
![image](https://github.com/user-attachments/assets/fc34e615-b5fd-473d-a8ff-497f501fe4aa)


* ### Next, we will Install ExpressJs and create the Routes directory.

# Install ExpressJS
* Express is framework for Node.js

* ### install Express using npm
```
npm install express
```
![image](https://github.com/user-attachments/assets/d110b006-50a7-4964-bcac-647c13ac77ca)

* ### create a file
```
touch index.js
```
check 
```
ls
```
install  the dotenv module

```
npm install dotenv
```
![image](https://github.com/user-attachments/assets/b30f935f-48d0-448d-ab2a-3cb55ff40f21)

* ### Open index.js file
```
vim index.js
```
copy and past

```
    const express = require('express');
    require('dotenv').config();
    
    const app = express();
    
    const port = process.env.PORT || 5000;
    app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "\*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    next();
    });
    
    app.use((req, res, next) => {
    res.send('Welcome to Express');
    });
    
    app.listen(port, () => {
    console.log(`Server running on port ${port}`)
    });
```

Run the commnad on terminal
```
node index.js
```
![image](https://github.com/user-attachments/assets/33e22e84-452e-48b1-8020-afb0432f9d28)

* ### Edit inbound rules:
![image](https://github.com/user-attachments/assets/689a09ef-b154-494a-b9fe-3c8d38d3a53b)

* ### Open Broswer 
```
http://<PublicIP-or-PublicDNS>:5000
```
![image](https://github.com/user-attachments/assets/8055ab76-a071-4410-9075-48389d1d7ed2)


* ### Create a folder routes
```
mkdir routes
```
* ### Change directory to routes folder.
```
cd routes
```
![image](https://github.com/user-attachments/assets/9ae5c9bf-faa6-410e-a990-b77603ebfc77)


* ### Now, create a file api.js with the command below
```
touch api.js
```
* ### Open the file with the command below
```
vim api.js
```
copy & past

```
const express = require('express');
const router = express.Router();

// GET request to retrieve all todos
router.get('/todos', (req, res, next) => {
  // Logic for handling the GET request goes here
});

// POST request to create a new todo
router.post('/todos', (req, res, next) => {
  // Logic for handling the POST request goes here
});

// DELETE request to delete a specific todo by ID
router.delete('/todos/:id', (req, res, next) => {
  // Logic for handling the DELETE request goes here
});

module.exports = router;
```

![image](https://github.com/user-attachments/assets/6075a6a3-0297-4604-bf24-486bed66f038)


* ### Moving forward let create Models directory.


# Models
* the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.
* A model is at the heart of JavaScript based applications, and it is what makes it interactive.
* We will also use models to define the database schema .
* To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.

*  ### Change directory back Todo folder with cd .. and install Mongoose

```
cd ..
```

```
npm install mongoose
```
![image](https://github.com/user-attachments/assets/d8efc941-47ec-4108-87ec-a08af0f311fa)



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

  Open the file todo.js
  ```
  vim todo.js
  ```

  copy & past

```
  const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Create schema for todo
const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
  }
});

// Create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

* Now we need to update our routes from the file api.js in 'routes' directory to make use of the new model.

* now Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit

```
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

// Get all todos (only return 'id' and 'action')
router.get('/todos', (req, res, next) => {
  Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next);
});

// Create a new todo
router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
  } else {
    res.status(400).json({ error: "The input field is empty" });
  }
});

// Delete a todo by id
router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({ _id: req.params.id })
    .then(data => res.json(data))
    .catch(next);
});

module.exports = router;
```



![image](https://github.com/user-attachments/assets/49da3efe-d170-4789-81b9-aff21ff6ea40)

  

* The next piece of our application will be the MongoDB Database

# MongoDB Database: