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
The next piece of our application will be the MongoDB Database

PR```
The next piece of our application will be the MongoDB Database

PR










