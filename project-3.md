## SIMPLE TO-DO APPLICATION ON MERN WEB STACK


In this project, I am tasked to implement a web solution based on MERN stack in AWS Cloud.
These are the steps i went through to complete this project;


1. Backend configuration
2. Install expressjs Models

3. Mongodb database creation
4. Frontend creation

### Backend Configuration


These are the code i used in this step


	` sudo apt install `


    	` sudo apt upgradegit`

 	` sudo apt-get install -y nodejs`

Install Node.js with the command above

	` sudo apt update`

*Application Code Setup*

	` mkdir Todo`

Next, you will use the command 

 	` npm init `   
     
     to initialise your project

![initialization of nodejs](./Images/initialization1.PNG)

### INSTALL EXPRESSJS  


To use express, install it using npm:

`npm install express`

Now create a file index.js with the command below

`touch index.js`

Run ls to confirm that your index.js file is successfully created

Install the dotenv module

`npm install dotenv`

Open the index.js file with the command below

`vim index.js`

Copy and paste this command inside the file
![Adding content to node.Js file](./Images/Indexjs.PNG)

open your server by typing :

`node index.js`

![node js is running](./Images/nodeindesjs.PNG)

![testing express.Js from the browser](./Images/ExpressJs.PNG)

The  POST, GET, DELETE task will be associated with some particular endpoint and will use different standard HTTP request methods for each task.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes

`mkdir routes &&cd routes &&touch api.js &&vim api.js`

Copy and paste 


![content of api.js file](./Images/vi.PNG)


Change directory back Todo folder and install Mongoose

`npm install mongoose`

also run this command below

`mkdir models && cd models && touch todo.js`

Open the file created with `vim todo.js` then paste the code below in the file:

![content of todo.js file](./Images/Moongoose.PNG)

Update our routes from the file api.js in ‘routes’ directory with the command below.

```const express = require ('express');
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

### MONGODB DATABASE
 I created an account with mongoose for nosql database
 and the connected my server

In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your Todo directory and name it .env.

`touch .env`

`vi .env`

Add the connection string to access the database in it, just as below:

```DB = mongodb+srv://samuel:samuel@sam.lgytw.mongodb.net/samuel?retryWrites=true&w=majority
```
Update the index.js to reflect the use of .env so that Node.js can connect to the database.

Open the file with `vim index.js` and add the code 
below

```const express = require('express');
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

Start your server using the command:

`node index.js`


## FRONTEND CREATION  

To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

`npx create-react-app client`

**Running a React App**
To run this react there are some dependencies that will catered for by the following commands 

`npm install concurrently --save-dev`

`npm install nodemon --save-dev`

replace 'script' part of the of the content of the 
package.json file with this codes below

``` 
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

Add the key value pair in the package.json file by using these codes below

`cd client`

`vi package.json`

` "proxy": "http://localhost:5000"`


To see the toto app enter the Todo directory, and simply do:
`npm run dev`
Your app should open and start running on 
          `http//localhost:3000`

**Creating your React Components**

In creating react components run the following codes

`cd client`
`cd src`
`mkdir components`
`cd components`

`touch Input.js ListTodo.js Todo.js`

Open Input.js file

`vi Input.js`

Copy and paste the following

```import React, { Component } from 'react';
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
Move to the src folder
`cd ..`

Move to clients folder
`cd ..`

Install Axios
`npm install axios`

## FRONTEND CREATION (CONTINUED)
 copy and paste the following code execute accordingly:

 `cd src/components`
`vi ListTodo.js`

```import React from 'react';

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
in the Todo.js file you write the following code

```import React, {Component} from 'react';
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
```

 `cd ..`



Copy and paste the code below into it

```import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}
```

export default App;

After pasting, exit the editor.

In the src directory open the App.css

`vi App.css`

Then paste the following code into App.css:

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

`vim index.css` 



enter in the src directory open the index.css 


```body {
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


` cd ../.. `

`npm run dev`

































