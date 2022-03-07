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
gi          `http//localhost:3000`


















