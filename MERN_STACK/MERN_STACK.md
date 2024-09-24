# MERN WEB STACK

# MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
MERN stands for:

- **M**ongoDB: A NoSQL database used for storing data in a flexible, document-oriented format.
- **E**xpress.js: A web application framework for Node.js, used for building APIs and handling server-side logic.
- **R**eact.js: A JavaScript library for building user interfaces, particularly for single-page applications.
- **N**ode.js: A JavaScript runtime that executes server-side code, allowing the development of scalable backend services.

  ![image](https://github.com/user-attachments/assets/1f574e94-8ba0-49b6-80ea-11cfb272f6b4)


Together, these technologies form a full-stack framework that enables developers to build modern web applications with a unified language—JavaScript—from the client to the server and the database.
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


![image](https://github.com/user-attachments/assets/d5ffa546-64ea-40b4-8e31-4b0937c5f5f1)

![image](https://github.com/user-attachments/assets/9c0f87ce-0a87-4c55-9fcd-ca4675995efb)

![image](https://github.com/user-attachments/assets/8bae3bb1-c999-4193-8c17-2f0fd53d797e)

. Install your driver
```
npm install mongodb
```
![image](https://github.com/user-attachments/assets/1e1ef415-4497-4e91-9c93-8f1ac5798064)


* In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

* ### Create a file in your Todo directory and name it .env.

```
touch .env
```

open file 
```
vi .env
```


* add the connection string to access the database in it, just as below:
```
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```


* Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.
* Simply delete existing content in the file, and update it with the entire code below.

* Open the file with vim index.js
* Press esc
* Type :Type %d
* Hit 'Enter'


Now, paste the entire code below in the file.

```
  const express = require('express');
  const bodyParser = require('body-parser');
  const mongoose = require('mongoose');
  const routes = require('./routes/api');
  const path = require('path');
  require('dotenv').config();
  
  const app = express();
  
  const port = process.env.PORT || 5000;
  
  //connect to the database
  mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log(`Database connected successfully`))
  .catch(err => console.log(err));
  
  //since mongoose promise is depreciated, we overide it with node's promise
  mongoose.Promise = global.Promise;
  
  app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "\*");
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
  console.log(`Server running on port ${port}`)
  });

```

![image](https://github.com/user-attachments/assets/c1064156-d3ad-4f2b-b54e-f16408568c2c)

* ### Start your server using the command:
```
node index.js
```


![image](https://github.com/user-attachments/assets/fd0c017a-04ad-4c53-bf78-882ec1453bd5)



* ## see a message 'Database connected successfully', if so - we have our backend configured. Now we are going to test it.

# Testing Backend Code without Frontend using RESTful API
* So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.

* In this project, we will use Postman to test our API. Click Install Postman to download and install postman on your machine.

* Click HERE to learn how perform CRUD operartions on Postman

* Now open your Postman, create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.

* note: make sure your set header key Content-Type as application/json


![image](https://github.com/user-attachments/assets/8716ffd0-8db2-4010-ba4d-45f2fd369b4b)


![image](https://github.com/user-attachments/assets/3dd55a45-ec7d-43c0-a0ef-a30c11570fb2)



*  Create a GET request to your API on http://<PublicIP-or-PublicDNS>:5000/api/todos. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

 ![image](https://github.com/user-attachments/assets/0713829e-7910-44b6-b0d9-ac0052c3e04d)

* check in Database
![image](https://github.com/user-attachments/assets/824c188b-b639-4295-9113-9222f9db023c)


# Step 2 - Frontend creation
* Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

* In the same root directory as your backend code, which is the Todo directory, run:

  ```
  npx create-react-app client
  ```
 ![image](https://github.com/user-attachments/assets/7e34c8e5-1c1a-4ef9-b29d-45961efb20b1)


  ![image](https://github.com/user-attachments/assets/a24b595c-66a1-475b-8a89-f74293bdfbf0)



  This will create a new folder in your Todo directory called client, where you will add all the react code.



* ### Running a React App
Before testing the react app, there are some dependencies that need to be installed.

Install concurrently. It is used to run more than one command simultaneously from the same terminal window.
```
npm install concurrently --save-dev
```





* Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
```
npm install nodemon --save-dev
```

![image](https://github.com/user-attachments/assets/6edb6965-4672-4f66-b5d6-2c9c5615afd3)




* In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.

```
"scripts": {
  "start": "node index.js",
  "start-watch": "nodemon index.js",
  "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
}
```


![image](https://github.com/user-attachments/assets/53d77e40-6ec3-4b6a-b02d-3de132738cbe)


* ### Configure Proxy in package.json
* Change directory to 'client'
```
cd client
```
* Open the package.json file
```
vi package.json
```

* Add the key value pair in the package.json file "proxy": "http://localhost:5000".

### Now, ensure you are inside the Todo directory, and simply do

```
npm run dev
```
![image](https://github.com/user-attachments/assets/075ad1be-8579-4244-be14-45062c703a12)

* ### The react app should load at http://localhost3000
* now view client in the browser, on network
![image](https://github.com/user-attachments/assets/6a7f355b-ec7c-480d-86bc-894a0743dd62)


### Creating your React Components

```
cd client
```
* move to the src directory
```
cd src
```
![image](https://github.com/user-attachments/assets/7cb0bd58-8a4b-405e-bcdf-f4a6ac26096c)


* Inside your src folder create another folder called components
```
mkdir components
```
![image](https://github.com/user-attachments/assets/bd6be461-8ad7-4c27-b374-b9d4b30e10e5)

* Move into the components directory with
```
cd components
```

* Inside 'components' directory create three files Input.js, ListTodo.js and Todo.js.

  ```
  touch Input.js ListTodo.js Todo.js
  ```
  * Open Input.js file
```
vi Input.js
```
* Copy and paste the following

```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: ""
  }

  handleChange = (event) => {
    this.setState({ action: event.target.value });
  }

  addTodo = () => {
    const task = { action: this.state.action };

    if (task.action && task.action.length > 0) {
      axios.post('/api/todos', task)
        .then(res => {
          if (res.data) {
            this.props.getTodos();
            this.setState({ action: "" });
          }
        })
        .catch(err => console.log(err));
    } else {
      console.log('Input field required');
    }
  }

  render() {
    let { action } = this.state;
    return (
      <div>
        <input type="text" onChange={this.handleChange} value={action} />
        <button onClick={this.addTodo}>add todo</button>
      </div>
    );
  }
}

export default Input
```

To make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the src folder, Move to clients folder and Install Axios

```
npm install axios
```
![image](https://github.com/user-attachments/assets/d0a6ffc2-398f-428a-8970-024846f71f80)



Go to components directory

```
cd src/components
```
After that we open the ListTodo.js

```
vi ListTodo.js
```

in the ListTodo.js we add the following code

```
  import React from 'react';

  const ListTodo = ({ todos, deleteTodo }) => {
  
  return (
  <ul>
  {
  todos &&
  todos.length > 0 ?
  (
  todos.map(todo => {
  return (
  <li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
  )
  })
  )
  :
  (
  <li>No todo(s) left</li>
  )
  }
  </ul>
  )
  }
  
  export default ListTodo
```
Then in our Todo.js file you write the following code

```
  import React, {Component} from 'react';
  import axios from 'axios';
  
  import Input from './Input';
  import ListTodo from './ListTodo';
  
  class Todo extends Component {
  
  state = {
  todos: []
  }
  
  componentDidMount(){
  this.getTodos();
  }
  
  getTodos = () => {
  axios.get('/api/todos')
  .then(res => {
  if(res.data){
  this.setState({
  todos: res.data
  })
  }
  })
  .catch(err => console.log(err))
  }
  
  deleteTodo = (id) => {
  
      axios.delete(`/api/todos/${id}`)
        .then(res => {
          if(res.data){
            this.getTodos()
          }
        })
        .catch(err => console.log(err))
  
  }
  
  render() {
  let { todos } = this.state;
  
      return(
        <div>
          <h1>My Todo(s)</h1>
          <Input getTodos={this.getTodos}/>
          <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
        </div>
      )
  
  }
  }
  
  export default Todo;
```

* We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this. Move to the src folder in the src folder we open the App.js and add the following code:

```
     import Todo from './components/Todo';
     import './App.css';
     
     const App = () => {
     return (
     <div className="App">
     <Todo />
     </div>
     );
     }
     
     export default App;
```


After pasting, exit the editor, in the src directory open the App.css, and then paste the following code into App.css:


```
.App {
  text-align: center;
  font-size: calc(10px + 2vmin);
  width: 60%;
  margin-left: auto;
  margin-right: auto;
}

input {
  height: 40px;
  width: 50%;
  border: none;
  border-bottom: 2px #101113 solid;
  background: none;
  font-size: 1.5rem;
  color: #787a80;
}

input:focus {
  outline: none;
}

button {
  width: 25%;
  height: 45px;
  border: none;
  margin-left: 10px;
  font-size: 25px;
  background: #101113;
  border-radius: 5px;
  color: #787a80;
  cursor: pointer;
}

button:focus {
  outline: none;
}

ul {
  list-style: none;
  text-align: left;
  padding: 15px;
  background: #171a1f;
  border-radius: 5px;
}

li {
  padding: 15px;
  font-size: 1.5rem;
  margin-bottom: 15px;
  background: #282c34;
  border-radius: 5px;
  overflow-wrap: break-word;
  cursor: pointer;
}

@media only screen and (min-width: 300px) {
  .App {
    width: 80%;
  }

  input {
    width: 100%;
  }

  button {
    width: 100%;
    margin-top: 15px;
    margin-left: 0;
  }
}

@media only screen and (min-width: 640px) {
  .App {
    width: 60%;
  }

  input {
    width: 50%;
  }

  button {
    width: 30%;
    margin-left: 10px;
    margin-top: 0;
  }
}
```


* In the src directory, open the index.css
```
vim index.css
```

copy & past
```
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}
```


* ### Go to the Todo directory

  ```
  cd ../..
  ```
* ###  Run:

```
npm run dev
```
![image](https://github.com/user-attachments/assets/f75d43dd-2812-4fc6-b38f-33275e9d40e5)

**To-DO APP** is ready 
open browser and check it
```
http://localhost:3000/
```

![image](https://github.com/user-attachments/assets/f0e303bd-f898-4aea-a702-98c657bc6580)


* ### Conclusion:

I successfully developed and deployed a To-Do application using the MERN stack. The frontend was built with React.js, allowing for an interactive user experience, while the backend was powered by Express.js, managing API requests and business logic. Data persistence was handled using MongoDB, where tasks were efficiently stored and managed. This project gave me valuable insights into full-stack development, API integration, and database management, strengthening my understanding of the MERN stack.






