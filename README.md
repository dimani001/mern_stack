# Overview of the MERN Stack

The MERN stack is a popular technology stack used for building dynamic and scalable web applications. MERN is an acronym that stands for MongoDB, Express.js, React.js, and Node.js. Each of these components provides a layer in the stack, handling everything from the database, server, and API layers to the frontend.

## Components of the MERN Stack

### MongoDB

- **Type**: NoSQL database
- **Use**: Stores data in flexible, JSON-like documents which allows varied data structures to be used.
- **Features**: High performance, high availability, and easy scalability.
- **Why Use**: It is schema-less, which makes it more flexible than traditional relational databases.
### Express.js

- **Type**: Web application framework
- **Use**: Simplifies the task of building server setups and routing, handling HTTP requests and middleware functionality with ease.
- **Features**: Robust routing, asynchronous programming, and integration with numerous middleware modules.
- **Why Use**: It’s minimal, scalable, and pairs seamlessly with Node.js.

### React.js

- **Type**: JavaScript library for building user interfaces
- **Use**: Constructs the front-end or client-side of the application.
- **Features**: Virtual DOM (for efficient updates), JSX (JavaScript XML for building components), and component-based architecture.
- **Why Use**: It enables developers to create large web applications that can change data, without reloading the page for every single state change.

### Node.js

- **Type**: JavaScript runtime environment
- **Use**: Allows you to run JavaScript on the server-side.
- **Features**: Non-blocking, event-driven architecture, capable of asynchronous I/O.
- **Why Use**: It’s lightweight, efficient, and its non-blocking I/O model makes it ideal for data-intensive real-time applications that run across distributed devices.

## Workflow of MERN Stack

1. **Client Requests**: The process begins with the client sending a request to the server, typically through a web interface created with React.js.
2. **Server Interaction**: Express.js running on Node.js handles the incoming request. It can interact with the database to create, read, update, or delete data.
3. **Database Operations**: MongoDB stores or retrieves data, which is then sent back to the server.
4. **Response Generation**: The server may perform additional processing based on the data retrieved or manipulated in the database before sending a response back to the client.
5. **Displaying Data**: React.js then takes this data and updates the view for the user, without needing a full page refresh.

##  Installing the MERN Stack on AWS EC2

These are the steps involved in the  process of setting up the MERN stack on an AWS EC2 instance.

### PART ONE

## Step 1: Set Up AWS EC2 Instance

1. **Log In to your AWS Console** and navigate to the EC2 dashboard.
2. **Launch an Instance**:
   - Select an Amazon Machine Image (AMI), like Ubuntu.
   - Choose an instance type, like `t3.micro`, which is free tier eligible.
   - Configure instance details, add storage (10 GB), and configure a security group(Launch-wizard -1) to allow traffic on ports 80 (HTTP), 443 (HTTPS), and 3000 (React) and 5000.
   - Launch the instance
   
![aws instance](https://github.com/user-attachments/assets/50feea96-d90c-4f9b-94a6-70cbcdefc307)

   ## Step 2: Connect to Your Instance

Connect to your EC2 instance using SSH (used gitbash on my terminal)

         cd "/c/Users/H.P I5 8TH GEN/Downloads"
         chmod 400 class2-publickey.pem
         ssh -i class2-publickey.pem ubuntu@13.49.57.31
         
   ![SSH'd into my instance](https://github.com/user-attachments/assets/f945a7d0-836b-4f7a-8158-50b701bed474)


 Update your Ubuntu Ec2 Instance

          sudo apt update && sudo apt upgrade
          
![sudo update and upgrade 2](https://github.com/user-attachments/assets/8d46f50a-120f-4728-b3c3-cbb598d86f37)

 install git, node js and npm (node package manager) 

         sudo apt install -y git curl
         curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
         sudo apt install -y nodejs
         
![node js   git installed (1)](https://github.com/user-attachments/assets/ec48f590-aec5-4257-b36f-221f086e0845)

Check the versions installed by running:

         node -v
         npm -v
         
![node js installed (2)](https://github.com/user-attachments/assets/946c0d8c-8588-410e-9352-3bf75a1ce5d7)

# Creating a Todo application
### this application will be able to
  * create new task
  * display a list of all tasks
  * delete completed tasks 

  ## step one :  Make a new directory, name it "todo" and move into it

         sudo mkdir todo
         cd todo

## step two :   Run an NPM init to create a package.json file

         npm init
Next, tap enter and follow instructions.

![npm init](https://github.com/user-attachments/assets/6635cce2-d82f-468e-96e1-0cc24b6a0c30)

## step three :  Install Express Js

         npm install express
         
![npm install 2](https://github.com/user-attachments/assets/aaa1a2b8-eeb4-46c6-be36-5a3aa8ddc4ba)

## step four : create an index.js file

         touch index.js

 ## step five :  Installing dotenv module

         npm install dotenv

## step six :  Open the index.js file , use nano or vim and write a simple code in it

            sudo nano index.js

```js
const express = require('express');
require('dotenv').config();

const app = express();

// Get port from environment or default to 5000
const port = process.env.PORT || 5000;

// Middleware to handle CORS
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

// Default route
app.get('/', (req, res) => {
  res.send('Welcome to Express');
});

// Start server and bind to all interfaces
app.listen(port, '0.0.0.0', () => {
  console.log(`Server running on port ${port}`);
});
```

## step seven : start server to see if it works

            node index.js

Make sure you allowed access from all ips to your port 5000 in your SG AWS console

then visit your ipaddess:5000

![express js output 7](https://github.com/user-attachments/assets/64f4ae75-05e1-4869-a38f-a633bb7fb091)

## step eight :  For each task, we need to create a routes for the different end points that the todo application will depend on

     mkdir routes
     cd routes

* create an api.js file in it

           touch api.js

* open the file using

           sudo nano api.js

* paste the code below

```js
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

});

module.exports = router;
```


## step nine: To be able to use mongodb, we need to install the mongoose package
            npm install mongoose
            
* Create a models directory

            mkdir models
            cd models

* Inside the models directory, we can create a file , lets call it todo.js

             touch todo.js

* open the file

             sudo nano todo.js

* paste this sample code in it


```javascript
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

## Step ten : Creating Mongodb  

   1. in your web browser, visit https://cloud.mongodb.com/
   2. Sign up for an account
   3. After sign up, click on Create Cluster button
   4. Choose AWS cloud and then region closest to you like South Africa
   5. click on create cluster

![mongo DB cloud](https://github.com/user-attachments/assets/2d926d6b-bf71-4e19-992b-656deaf92e84)


### Configure Security Settings
   1.  Create a Database User:
   2.  Go to the “Database Access” under the “Security” section.
   3.  Click on “Add New Database User”.
   4.  Enter a username and password. Ensure the “Read and Write to any database” option is selected.
   5.  Click "Add User".
   6.  Then go back to cluster page, click in connect, select drivers and choose Mongoose , copy your conection url

![Mongo Database](https://github.com/user-attachments/assets/d2ea4caa-49e2-4a5c-8351-723ada9037f4)

## step eleven :  Connecting The App to Mongo db

   1. create a .env file in the todo directory and open the file;

            touch .env
            sudo nano .env

   NOTE: creating .env variables to store information is a very secure practice is highly recommended

   2. Create a connection string

            DB=mongodb+srv://chidimma516:1uM7NvW4a6bFmxgp@main-db.avhxcwg.mongodb.net/todo?retryWrites=true&w=majority&appName=Main-DB PORT=5000

      
      edit and add your own <username: your usernamee, <password: your password > where necessary

  3.  Whitelist Your EC2 Public IP Address on MongoDB Atlas

Get your EC2 public IP:

             curl ifconfig.me
    
Next, On the https://cloud.mongodb.com/ interface,
    
Navigate to:

* Your Project
* Network Access (on the left sidebar)
* Click “+ ADD IP ADDRESS”
*  Paste your EC2 public IP address
*  Click "Confirm".

![Mongo Network Access](https://github.com/user-attachments/assets/0ca8df23-900f-4314-8662-2becf31d8ea2)

 4. Navigate to the index.js file which i already set up in todo directory

               sudo nano todo/index.js

5. Clear out previous code and paste this sample :
# Backend Server (index.js)


```javascript
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();
const port = process.env.PORT || 5000;

// Connect to MongoDB
mongoose.connect(process.env.DB, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => console.log('✅ Database connected successfully'))
.catch(err => console.error('❌ Error connecting to MongoDB:', err));

// Use native promises
mongoose.Promise = global.Promise;

// CORS middleware
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

// Body parser middleware
app.use(bodyParser.json());

// API routes
app.use('/api', routes);

// Error-handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send({ error: err.message });
});

// Start the server
app.listen(port, '0.0.0.0', () => {
  console.log(`🚀 Server running on port ${port}`);
});
```

 4. Start the server

                 node index.js
   
   ![DB successful](https://github.com/user-attachments/assets/2537c447-259e-45d5-a3c8-b203f0d7f646)


 ## PART TWO

 *connecting the backend code using restful api

   1.   download postman software from   https://www.postman.com/downloads/
   2.   Open Postman and create an api request to <your-server-IP:5000/api/todos?>
   3.   Set the HTTP method to POST
   4.   set the header : key=  content-tyep, Value = application/json
   5.   Now create a GET request by  Clicking on http, choose GET, add the previous address from the post request
   6.   To create a DELETE request,  click on http, choose DELETE, add the same address as we did in GET and POST request

![postman png](https://github.com/user-attachments/assets/8bce5ef5-19ae-4a81-b5d1-770dcd918c8d)


*  Lets create a front end , this is the Client interface that will interact with the api

     1. Naviage to todo directory

              cd todo

     2. Create a react project
               npx create-react-app client

     3. Install concurrently, as well as nodeman
       concurrently is used to run multiple commands at the same time
 
               npm install concurrently --save-dev

     4.  Nodemon is used to run and monitor the server, in order to reload automatically if it detects any change

               npm install nodemon --save-dev

     5.  Set it up by navigating to pacakge.json in the root directory

               sudo nano package.json
 Paste this instead

```json
{
  "name": "todo",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "start-watch": "nodemon index.js",
    "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "dotenv": "^16.5.0",
    "express": "^5.1.0",
    "mongoose": "^8.15.1"
  },
  "devDependencies": {
    "concurrently": "^9.1.2",
    "nodemon": "^3.0.2"
  }
}
```

   6. Navigate back to todo directory and run

                   npm run dev

   * the react app should load at http://youripaddress:3000
   *  Your backend server should load at http://youripaddress:5000
 
   *  P>S ensure to allow port 3000 in aws in bound rules
    
![react script](https://github.com/user-attachments/assets/ff1f7f83-bfe7-437f-bf2b-1ed37d55a61b)![React png](https://github.com/user-attachments/assets/8cc1357a-78f4-41ce-9163-89178b9ebfa6)

  7. Navigate to src directory and create new directory "components" , then create 3 files "Todo.js, Input.js, ListTodo.js"

                 cd client/src           # Navigate to the React source directory
                 mkdir components        # Create a new folder named 'components'
                 cd components           # Enter the components folder
                 touch Input.js Todo.js ListTodo.js   # Create the three component files

     8. now open each of those files and paste these codes
                 * open Todo.js

                 sudo nano Todo.js

        input these codes samples

```jsx
import React, { Component } from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {
  state = {
    todos: []
  }

  componentDidMount() {
    this.getTodos();
  }

  getTodos = () => {
    axios.get('/api/todos')
      .then(res => {
        if (res.data) {
          this.setState({
            todos: res.data
          });
        }
      })
      .catch(err => console.log(err));
  }

  deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if (res.data) {
          this.getTodos();
        }
      })
      .catch(err => console.log(err));
  }

  render() {
    let { todos } = this.state;

    return (
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos} />
        <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
      </div>
    );
  }
}

export default Todo;
```


 * open ListTodo.js

            sudo nano ToDo.js

    paste this sample codes

```jsx
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {
  return (
    <ul>
      {
        todos && todos.length > 0 ? (
          todos.map(todo => (
            <li key={todo._id} onClick={() => deleteTodo(todo._id)}>
              {todo.action}
            </li>
          ))
        ) : (
          <li>No todo(s) left</li>
        )
      }
    </ul>
  );
};

export default ListTodo;
```


 *  Open Input.js

            sudo nano Input.js
    paste this code

```jsx
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: ""
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
      console.log('input field required');
    }
  }

  handleChange = (e) => {
    this.setState({
      action: e.target.value
    });
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
```
export default Input;

 8. Navigate back to clients directory and install axios. Axios is a promised based HTTP library that enables developers make request to thier own server, or third party servers

            cd ../..
            npm install axios

    9.  Move back into the src directory and edit the app.js file to remove the defualt react logo

               cd src
               sudo nano src/App.js

       paste this code in it

```jsx
import React from 'react';
import Todo from './components/Todo';
import './App.css';

function App() {
  return (
    <div className="App">
      <Todo />
    </div>
  );
}
```

export default App;


 * Open the app.css also and paste this code

          cd ~/todo/client/src
          sudo nano App.css

*  Remove any existing styles and paste the full CSS you provided:

```css
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


 * Open the index.css file and paste this code in it

           cd ~/todo/client/src
           sudo nano index.css

*  Remove any existing content, then paste this CSS code:

```css
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
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


10. Navigate back into parent directory and run the build using this command

              npm run dev

React build would compile sucessfully and both backend and front end will run concurrently through the ports we earlier opened

git

![todo server](https://github.com/user-attachments/assets/836640b2-eb88-4c7d-9897-8afb600abe13)
![todo output](https://github.com/user-attachments/assets/8f3ab6c2-6b52-4b91-b433-0e0ed009c749)




### CONCLUSION

We have just created a todo web application , front end based on react and backend is based on the Express js framework, alos we have been able to create api using POSTMAN,
Over all, we have learnt how to deploy a MERN stack into our cloud server
# mern_stack
