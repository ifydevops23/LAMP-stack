# MERN STACK DEPLOYMENT 

## SIMPLE TO-DO APPLICATION ON MERN WEB STACK
In this project, I implemented a web solution based on MERN stack in AWS Cloud.

MERN Web stack consists of following components:
- MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
- ExpressJS: A server-side Web Application framework for Node.js.
- ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
- Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

A user interacts with the ReactJS UI components at the application front-end residing in the browser. This frontend is served by the application backend residing in a server, through ExpressJS running on top of NodeJS.

## STEP 0 - PREREQUISITE
- An Ubuntu VM
- SSH Client (E.g Putty)

![1_EC2_VM](https://github.com/ifydevops23/Software_Stack/assets/126971054/24e2750b-fd67-49f7-9953-6bfbc8ff631e)

STEP 1 – BACKEND CONFIGURATION
Update ubuntu
`sudo apt update`
Upgrade ubuntu
`sudo apt upgrade`

![1_update_upgrade](https://github.com/ifydevops23/Software_Stack/assets/126971054/722d8156-58ee-467e-8c11-62ff1a17cda1)

Download Node.js software from Ubuntu repositories.
`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - `

![1_nodeSource_script](https://github.com/ifydevops23/Software_Stack/assets/126971054/16cc9901-c05a-4376-b6ea-77dcc7ad1c88)

Install Node.js on the server
`sudo apt-get install -y nodejs`

The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.
Verify the node installation with the command below
`node -v`
Verify the node installation with the command below
`npm -v` 

![1_node_install](https://github.com/ifydevops23/Software_Stack/assets/126971054/826c90ad-4ddf-4285-afd3-8e5f5a2da816)

### Application Code Setup
Create a new directory for your To-Do project:
`mkdir Todo`
Now change your current directory to the newly created one:
`cd Todo`
Next, use the command npm init to initialise your project, so that a new file named **package.json** will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. Press Enter several times to accept default values, then accept to write out the package.json file by typing yes.
`npm init`

![1_npm_init](https://github.com/ifydevops23/Software_Stack/assets/126971054/941599de-36c4-46ab-b47e-c310f8a63eef)

Run the command `ls` to confirm that you have package.json file created.
Next, we Install ExpressJs and create the Routes directory.

### Install ExpressJS

Install ExpressJS
Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore, it simplifies development, and abstracts a lot of low-level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

To use express, install it using npm:
`npm install express`

![1_npm_install_express](https://github.com/ifydevops23/Software_Stack/assets/126971054/8f8d1976-9622-4868-aece-3ae03458e622)

Now create a file index.js with the command below
`touch index.js`
Run `ls` to confirm that your index.js file is successfully created
Install the dotenv module
`npm install dotenv`

![1_dotenv_install](https://github.com/ifydevops23/Software_Stack/assets/126971054/ac867a8a-d0fe-4c42-b11f-8720d6ace2d6)

Open the index.js file with the command below
`vim index.js`
Type the code below into it and save. 

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

Notice that port 5000 is used in the code. This will be required later when we go on the browser.

We start our server to see if it works. Open your terminal in the **Todo** directory  where the  index.js file is located and type:
`node index.js`

![1_node_server_running](https://github.com/ifydevops23/Software_Stack/assets/126971054/4d9afec2-5bf6-4519-9c4a-cc84304c487a)

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:
`http://<PublicIP-or-PublicDNS>:5000`

![Welcome_to_express](https://github.com/ifydevops23/Software_Stack/assets/126971054/356a8bd9-d182-4712-af2a-95a2da92c0b9)

**Routes**

- There are three actions that our To-Do application does:
1. Create a new task
2. Display list of all tasks
3. Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.
For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes.
`mkdir routes`
Change directory to routes folder.
`cd routes`
Create a file api.js with the command below
`touch api.js`
Open the file with the command below
`vim api.js`
Copy below code in the file. 

```
const express = require ('express');
const router = express.Router();
 
router.get('/todos', (req, res, next) => {
 
});
 
router.post('/todos', (req, res, next) => {
 
});
 
router.delete('/todos/:id', (req, res, next) => {
 
})
 
module.exports = router;
```

Next, Create Models directory.

### Models
Since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.
A model is at the heart of JavaScript based applications, and it is what makes it interactive.
We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document.
In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.
Change directory back Todo folder with cd .. and install Mongoose
`npm install mongoose`

![1_insatll_mongoose](https://github.com/ifydevops23/Software_Stack/assets/126971054/d7d30693-7a51-4efd-9b81-1007845ce6f2)

Create a new folder models:
`mkdir models`
Change directory into the newly created ‘models’ folder with
`cd models`
Inside the models folder, create a file and name it todo.js
`touch todo.js`

Open the file created with `vim todo.js` then paste the code below in the file:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
 
//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})
 
//create model for todo
const Todo = mongoose.model('todo', TodoSchema);
 
module.exports = Todo;
```

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with `vim api.js` delete the code inside with :%d command and paste there code below into it then save and exit

```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');
 
router.get('/todos', (req, res, next) => {
 
//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});
 
router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});
 
router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})
 
module.exports = router;
```

The next piece of our application will be the MongoDB Database

**MongoDB Database**
MongoDB Database
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS).
Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

- IMPORTANT NOTE
- Change the time of deleting the entry from 6 Hours to 1 Week

Create a MongoDB database and collection inside mLab

![1_create_db](https://github.com/ifydevops23/Software_Stack/assets/126971054/2dc37bba-8521-4ec9-b37f-85abc6c734f8)

Create DB user
![1_add_db_user](https://github.com/ifydevops23/Software_Stack/assets/126971054/e8af0df5-7de8-44c9-b22f-fe59c64892c0)

Allows Access from Anywhere
![1_add_ip_address](https://github.com/ifydevops23/Software_Stack/assets/126971054/ed25702c-7b1d-49e9-b21d-af585450cc5d)

![1_ip_from_anywhre](https://github.com/ifydevops23/Software_Stack/assets/126971054/77251bd2-f2e6-4525-b20d-e651e1e16677)

In the index.js file, we specified process.env to access environment variables. So we need to do that now.

Create a file in your Todo directory and name it .env.
`touch .env`
`vi .env`
Add the connection string to access the database in it, just as below:
`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`
Ensure to update <username>, <password>, <network-address> and <database> according to your setup
 
Here is how to get your connection string
 
![1_connect_to_db](https://github.com/ifydevops23/Software_Stack/assets/126971054/f71dfca3-cc10-40bf-827c-f5827d15458b)

Update the index.js to reflect the use of .env so that Node.js can connect to the database.

Open the file with `vim index.js`
Paste the entire code below in the file.
 
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

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.
 
Start your server using the command:
`node index.js`

![1_db_connected](https://github.com/ifydevops23/Software_Stack/assets/126971054/854c655f-c6c7-45d3-9094-ce89ee7bcbd9)

**Testing Backend Code without Frontend using RESTful API**
 
During development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.
In this project, I used Postman to test our API.
Test all the API endpoints and ensurere they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.

Create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.
Note: make sure your set header key Content-Type as application/json

![1_postman_post](https://github.com/ifydevops23/Software_Stack/assets/126971054/5fe173ad-f0f3-42c0-a8ff-a8a7e665a75f)
 

Check the image below:

Create a GET request to your API on http://<PublicIP-or-PublicDNS>:5000/api/todos. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

![1_postman_get](https://github.com/ifydevops23/Software_Stack/assets/126971054/551ddde6-9180-4731-8906-5db9f20ac262)
 
## STEP 2 – FRONTEND CREATION
We are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

 In the same root directory as your backend code, which is the Todo directory, run:
 `npx create-react-app client`
 
This will create a new folder in your Todo directory called client, where you will add all the react code.

**Running a React App**
Before testing the react app, there are some dependencies that need to be installed.
Install concurrently. It is used to run more than one command simultaneously from the same terminal window.
`npm install concurrently --save-dev`
Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
`npm install nodemon --save-dev`
In Todo folder open the package.json file and update the scripts section:
 
```
"scripts": {
  "start": "node index.js",
  "start-watch": "nodemon index.js",
  "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

![1_adjust_script_for_reactDependencies](https://github.com/ifydevops23/Software_Stack/assets/126971054/3a4bad22-76d9-4a86-9229-4b7c686e4220)

Configure Proxy in package.json
Change directory to ‘client’
`cd client`
Open the package.json file
`vi package.json`
Add the key value pair in the package.json file "proxy": "http://localhost:5000"
The whole purpose of adding the proxy configuration above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos
 
Now, ensure you are inside the Todo directory, and simply do:
`npm run dev`
Your app should open and start running on localhost:3000
![1_react_app_from_web](https://github.com/ifydevops23/Software_Stack/assets/126971054/3c5f941b-f7b1-416e-aaab-229f1018c2e4)

**Important note: In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule.**
 
**Creating your React Components**
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.

 From your Todo directory run
`cd client`
Move to the src directory
`cd src`
Inside the src folder create another folder called components
`mkdir components`
Move into the components directory with
`cd components`
Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.
touch Input.js ListTodo.js Todo.js
 
![1_create_files_in_components](https://github.com/ifydevops23/Software_Stack/assets/126971054/9f9551cd-f72a-4f32-835f-8333a229593f)

Open Input.js file
`vi Input.js`
Copy and paste the following

```
import React, { Component } from 'react';
import axios from 'axios';
 
class Input extends Component {
 
state = {
action: ""
}
 
addTodo = () => {
const task = {action: this.state.action}
 
    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }
 
}
 
handleChange = (e) => {
this.setState({
action: e.target.value
})
}
 
render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
```
 
To make use of Axios, which is a Promise based HTTP client for the browser and node.js, change directory cd into  "client" from your terminal and run yarn add axios or npm install axios.
Move to the src folder
`cd ..`
Move to clients folder
`cd ..`
Install Axios
`npm install axios`

Go to ‘components’ directory
`cd src/components`
Open your ListTodo.js
vi ListTodo.js
In the ListTodo.js copy and paste the following code

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

Open Todo.js file you write the following code

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

Make adjustment to our react code. Delete the logo and adjust the App.js.
Move to the src folder
`cd ..`
Make sure that you are in the src folder and run
`vi App.js`
Copy and paste the code below into it
```
import React from 'react';
 
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
 
After pasting, exit the editor.
In the src directory open the App.css
`vi App.css`
Paste the following code into App.css:
 
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
width: 100%
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

In the src directory open the index.css
`vim index.css`
 
Copy and paste the code below:
```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}
 
code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```
Go to the Todo directory
cd ../..
In the Todo directory run:
`npm run dev`
 
![2_devServer_succesful](https://github.com/ifydevops23/Software_Stack/assets/126971054/8b17bc7b-c5f6-429f-a02b-b46b32f90ad6)

 
Our To-Do app is ready and fully functional with the functionality of: creating a task, deleting a task and viewing all tasks.

![2_snip_after_delete_of_tasks](https://github.com/ifydevops23/Software_Stack/assets/126971054/7261e005-04af-4663-99cd-bf5940e85602)

In this, I made a simple To-Do and deployed it to MERN stack. Wrote a frontend application using React.js that communicates with a backend application written using Expressjs. i also created a MongoDB backend for storing tasks in a database.

