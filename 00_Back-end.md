
# Back-end using MySQL, NodeJS and Express

## MySQL

At this point of the Lab Sessions, I assume that you have experience using MySQL and phpMyAdmin. So We will skip that part and let you creat your own models.
As the intention of these set of tutorials is to replicate the funcionality of your ***codeigniter project*** we will consume the your existing DB but we will create our own CRUD operations using NodeJS and ExpressJS.

## NodeJS

- Node allows developers to write JavaScript code that runs directly in a computer process itself instead of in a browser.
- Node provides access to several important global objects for use with Node program files (module, requiere, process).
- Node has a many built-in modules to aid in interactions with the command line, the computer file system, and the Internet.
- Node gives you the ablity to install packages that were built by other developers

[More info](https://www.codecademy.com/articles/what-is-node)

### Installation

 [Official instructions](https://nodejs.org/en/)

## Creating a simple [server](https://en.wikipedia.org/wiki/Server_(computing)) with NodeJS

1. Create a folder named Server (wherever you want, just remember where...)

2. Inside the Server folder create an empty document and name it SimpleServer.js (name of file does not matter but the extension does)

3. Open SimpleServer.js with your favorite IDE (I recommend to use Visual Studio Code)

4. In the first line of the document simply write

```javascript
var http =require('http'); // read buil-in module
```

5. Create an object from the module we are importing in the first line

```javascript
http.createServer((req,res)=>{
res.write("This line has been instantiated from the server...")
res.end()
}).listen(8000)
```

6. So far nothing has happend since we haven't told Node.js to start the server. In order to do it: i) open the terminal/console app and ii) run the comand:

```console
node path/to/your/SimpleServer.js
```

- Now you can see that the console is in kind of frozen state, but it is actually working.

7. In order to see what is going on, open your favorite browser and type in the url bar: <http://localhost:8000/>. Be sure that the ending point of the url is :8000, since that is the port we have asasigned to listen and echo requests.

8. The **client** is, in this case running in a browser (Chrome,  Safari, Firefor,  etc.). As the client is a browser, it can "understand" HTML and JavaScript. So let's see what we can do.

```javascript
res.write("This line has been instantiated from the server...")
```

- And chang it for:

```javascript
res.write("<h1>This line has been instantiated from the server...</h1>")
```

- For some of you, maybe the previous example didn't work. This is becuase your browser needs an specific "instruction". 

``` javascript
res.writeHead(200,{"Content-Type": "text/html"})
res.write("<h1>This line has been instantiated from the server...</h1>")
```

**IMPORTANT, SKIP STEPS 9-11 IF YOU ARE DOING THIS IN FAMNIT SERVER**

9. Now as you can see, every time we make a modification in the server we have to stop node and run it again as in the step 6. Here is when the magic of Node start to happen. As  you might recall, Node.js allows you to install third party (modules, API, functions and more) thourgh package manager. In this case, we will install [nodemon](https://www.npmjs.com/package/nodemon), a tool that automatically restarts the node application when changes in the directory and/or files are detected. Type this in the console/terminal application of your OS, be sure that you are in the root of your Directory.

```console
npm install nodemon
```

- if you are in OSX, try this instead:

```console
sudo npm install -g --force nodemon
```

- if you are in Windows, try opening a PowerShell console as Admin and type the following commands:

```console
Set-ExecutionPolicy RemoteSigned
Set-ExecutionPolicy Unrestricted
Get-ExecutionPolicy
Exit
```

10. If everything went well, now we have next to our SimpleServer.js file 3 new items: i) a folder named: node_modules, and ii) two files, namely: packege-lock.json and package.json.

11. For this step, instead of running our SimpleServer.js using the code from step 6, let's do it like this.

```console
nodemon path/to/your/SimpleServer.js
```

- Try to change something to see that the server is refreshed everytime there is a change in the code.
- Using nodemon during developing time might save you a lot of time :)
- Finally, for this simple server, let's handle requests from the client.

12. Insert this line in the code:

```javascript
res.write("\nUser is in: " + req.url)
```

- Try to put this URL in the browser <http://localhost:8000/Novice>
- As you can observe, now we are able two handle routes in case our web app has different pages.
- If you read the documentation you will see that there are many ways to handla request an it's parameters. Things could get really complicated if we for example, have many routes, models, and exchange constantly information between server and client. The good news are that NodeJS has a lot of dependencies/frameworks that we can use in order to reduce our coding ;).
- So stop your currener server if running, save you files and continue to the next part fo this tutorial.

## Express.js

### What is Express

- Express is a middleware that helps us to deal with server-side logic for web and mobile applications
- It's easy to use and plays along with many other frameworks, such as react, mongo,angular, etc.
- It's javascript :)

### Creating a NodeJs + Express.js server

We will learn about *ExpressJS* by building something. In this case, We will replicate the functionality of your *codeigniter project*. So, In order to do this I have subdivided this next part of the tutorial in 4 pieces, namely:

- [The server](#the-server)
- [The routes](#the-routes)
- [The DB](#the-db)
- [The CRUD](#the-crud)

Be aware that you have to do all of them in order.

### The server

1. Clone this [repository](https://gitlab.com/famnit-up/systems-iii/frameworks/-/tree/main)  and create a folder at the root lavel and name it *CMS* (short for Content Managment System)

2. Navigate inside the folder and run the command

```console
npm init
```

- By pressing enter everytime console prompts, you will keep the default values if you don't modify the entries
- The outcome of this operation will be a file named *package.json* that contains the basic information of our NodeJS project, particularlly, the packages/dependecies that we have installed so far, none.

3. Install your first dependency by running the command:

```console
npm install express --save
```

4. In the root of this project, create a javascript empty file, by convention you should name it **index.js** but it does't really matter.

5. Open your **index.js**  in your favorite IDE and in the first line import the recently installed dependency

```javascript
const express = require('express')
```

- note that in the new versions of javascript is not needed to finish the statments using semicolon :)
- if you are not familiar with type of variables in JS, review this article <https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/>

6. Create an instace of *ExpressJS* like this.

```javascript
const app = express()
```

7. Create a simple route for the server and assign a port to listen

```javascript
const port = 5000

app.get("/",(req,res)=>{
res.send("hola")
})

///App listening on port
app.listen(process.env.PORT || port, ()=>{
console.log(`Server is running on port: ${process.env.PORT || port}`)
})
```

- Observe that, first we select in wich port we want to listen int he declaration *const port*.
- then in *app.get*, we have passed 2 parameters. The first one that defines the route tha servers is "expecting", in this case "/", this means "http://ADDRESS:5000". The second one a callback to handle the request and response. If you remember, it is  in the same way we did when creted the simple server. In this case if someone visits the url, we will send the string hola to the browser.
- finally,  in *app.listen*, we instruct our server to start listening in a dedicated port, in this case 5000.

8. Save the file and in the console run

```console
node path/to/your/index.js
```

- Everythis it's ok you will be able to see in the browser the string "hola"
- We could write all our logic in this ***index.js*** file, but at some point it's gona become messy so let's instead create some *routes* in different files using the power of *ExpressJS*.

### The routes

1. In your *CMS* folder create a new folder and name it: *routes*.

2. Inside *routes* folder create an empty javascript document and name it: *novice.js*.

3. In the *novice.js* file, import express and then create an instance of express.Router

```javascript
const express= require("express")
const novice = express.Router();
```

- We will see how it work once we define our first route

5. Create a route that points to *"/"*

```javascript
novice.get('/',(req,res)=>{
console.log("The route has been reached")
res.send("novice")
})
```

6. Export your module so it can be called from other script by putting at the end of the code.

```javascript
module.exports=novice
```

7. In **index.js**, import **novice.js** and then use it like this.

```javascript
//Import opur custom modules-controllers
const novice= require("./routes/novice")
//Routes
app.use('/novice', novice);
```

8. Save the file and in the console run:

```console
node path/to/your/index.js
```

9. In the browser visit *http://ADDRESS:5000/*

- if you have follow each step so far you should be able to see a message in the console and the string *novice* in the browser
- The next step in this tutorial consists in consuming the information from the DB you created in *codeigniter project*. So every time an user visits an specific *endpoint*, for instace, http://ADDRESS:5000/novice, we get the news from the DB.
- The idea  for next steps is: i) to stablish aconnection with the DB and then ii) create our own CRUD opperations.

### The DB

1. In the root of our CMS project, create a folder and name it DB.

2. Create an empty file in DB folder and named it **dbConn.js**

3. Install the following dependencies:

```console
npm install mysql2 dotenv
```

- The first one (mysql2) is a dependency that will help us to deal with the connection with the data base and the second one to keep secret our data base information

4. In **DBConn.js**, import *ExpresJS*, justa as in step **5** of **The server**

6. Then define a variable tha will hold the connection:

```javascript
const  conn = mysql.createConnection({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASS, 
    database: 'Qcodeigniter',
  })

 conn.connect((err) => {
      if(err){
          console.log("ERROR: " + err.message);
          return;    
      }
      console.log('Connection established');
    })
  
```

- In your case the value of the *database* key must contain excactly the same name as your database.
- In the next step we will define the *host*, *user* and *password*.
- Observe that provisonally I have put a block of code to check if the dependency managed to get connected to the dababase, for now let's just save.

7. In the CMS folder, create a file a name it *.env*, open it and write:

```text
DB_HOST=the address of your database
DB_USER=the user from your data base
DB_PASS=your password
```

- As we want to keep this information privatley and not to expose it in your repository, let's also create an *.gitignore* file in CMS folder and simply put

```text
.env
```

8. Finally to test if it works, go to  **index.js** and add this line after the line where you imported *ExpressJS*:

```javascript
//Basic packages
const express = require('express')
require('dotenv').config()
```

9. Run the code as ussuall with *node index.js*
-if everything ok, we should be able to see the message in the console *Connection established*
-if not, check each step and be sure that your are using the right credential and configurations, and have imported all requiered dependencies.

### The CRUD
For this part it is important that you understand the logic of the server and how this interacts with the routes and the database.A technique to do this is to read the code and sketch app flow.
So far we have learned how to create a server that is listening for incoming request, how to create a route to handle requests on an specific endpoint and lastly how to stablish a connection with the database. 

Now it's time to expand our application and write the code for Consulting, Reading, Updating and Deleting data from our database.

1. In **dbConn.js** add the piece of code to handle the CRUD:
```javascript
let dataPool={}
  
dataPool.allNovice=()=>{
  return new Promise ((resolve, reject)=>{
    conn.query(`SELECT * FROM news`, (err,res)=>{
      if(err){return reject(err)}
      return resolve(res)
    })
  })
}

dataPool.oneNovica=(id)=>{
  return new Promise ((resolve, reject)=>{
    conn.query(`SELECT * FROM news WHERE id = ?`, id, (err,res)=>{
      if(err){return reject(err)}
      return resolve(res)
    })
  })
}

dataPool.creteNovica=(title,slug,text)=>{
  return new Promise ((resolve, reject)=>{
    conn.query(`INSERT INTO news (title,slug,text) VALUES (?,?,?)`, [title, slug, text], (err,res)=>{
      if(err){return reject(err)}
      return resolve(res)
    })
  })
}

```

- Note that I have created a variable of type object that will store the data returned after every query.
- The returned data is callback from a given function. In this case the function is returning a *[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)*.
- As you may noticed, the *Promise* is a function that will return either an error if the request failed or a resloution if data query was fullfiled.
- What is the data that needs to be resolved or return as error? The one we are requestin in the *conn.query()*
- Data in between quotes looks familiar? It's because this is the SQL instruction that you learned how to use some Lab Session behind. 
- To keep things short I have also implemented a request for a single new *oneNovica* that it's pretty similar to the first function but I have included and identifier to overload the request.
- The last function *createNovica* follows a similar structure to *oneNoica* but receives 3 parameters instead. This is because we defined our database to autoincrement the id counter everytime a new article is inserted. 
- Analyse previouse code and keep it in mind just in case you need to implement something similar later on ;)
2. Add the queries related with the user loging/registration
```javascript
dataPool.AuthUser=(username)=>
{
  return new Promise ((resolve, reject)=>{
    conn.query('SELECT * FROM user_login WHERE user_name = ?', username, (err,res, fields)=>{
      if(err){return reject(err)}
      return resolve(res)
    })
  })  
	
}

dataPool.AddUser=(username,email,password)=>{
  return new Promise ((resolve, reject)=>{
    conn.query(`INSERT INTO user_login (user_name,user_email,user_password) VALUES (?,?,?)`, [username, email, password], (err,res)=>{
      if(err){return reject(err)}
      return resolve(res)
    })
  })
}
```
- In some sense they follow the same formula as the previous ones for news but in this case they handle data associated with the user.
3. Open **novice.js** and repleace the all previous code for the following:

```javascript
const express= require("express")
const novice = express.Router();
const DB=require('../DB/dbConn.js')

//Gets all the news in the DB 
novice.get('/', async (req,res, next)=>{
    try{
        var queryResult=await DB.allNovice();
        res.json(queryResult)
    }
    catch(err){
        console.log(err)
        res.sendStatus(500)
    }
})

//Gets one new based on the id 
 novice.get('/:id', async (req,res, next)=>{
    try{
        var queryResult=await DB.oneNovica(req.params.id)
        res.json(queryResult)
    }
    catch(err){
        console.log(err)
        res.sendStatus(500)
    }
}) 

//Inserts one new to the database
novice.post('/', async (req,res, next)=>{
        
  let title = req.body.title
  let slug = req.body.slug
  let text = req.body.text

    var isAcompleteNovica=title && slug && text
    if (isAcompleteNovica)
    {
        try{
            var queryResult=await DB.creteNovica(title,slug,text)
            if (queryResult.affectedRows) {
                console.log("New article added!!")
              }
        }
        catch(err){
            console.log(err)
            res.sendStatus(500)
        }    
    }  
    else
    {
     console.log("A field is empty!!")
    }
    res.end()

  
}) 
module.exports=novice
```
- Observe than the first two routes are using get and the last one the post method. I hope you still rremember your previous Lab Sessions :)
- All the request need to be revised before invoke the methods we created to consult the database.
- try and catch are gonna help us in this case break the application if something goes worng during running time. Let's do the proper for users.
4. Create a new file  in **routes** folder and name it **users.js** then copy and paste the following code:
```javascript
const express= require("express")
const users = express.Router();
const DB=require('../DB/dbConn.js')

//Checks if user submited both fields, if user exist and if the combiation of user and password matches
users.post('/login', async (req, res) => {
    var username = req.body.username;
	var password = req.body.password;
    if (username && password) 
    {
        try
        {
         let queryResult=await DB.AuthUser(username);
        
                if(queryResult.length>0)
                {
                    if(password===queryResult[0].user_password)
                    {
                    req.session.user=queryResult;
                    console.log(req.session.user)
                     console.log(queryResult)
                     console.log("SESSION VALID");
                    
                     //res.redirect('/');
                    }
                    else
                    {
                        console.log("INCORRECT PASSWORD");
                    }
                }else 
                {
                 console.log("USER NOT REGISTRED");   
                }
        }
        catch(err){
            console.log(err)
            res.sendStatus(500)
        }    
    }
    else
    {
        console.log("Please enter Username and Password!")
    }
    res.end();
});

//Inserts a new user in our database id field are complete
users.post('/register', async (req, res) => {
    
    let username = req.body.username
	let password = req.body.password
    let email= req.body.email
    if (username && password && email) 
    {
        try
        {
         let queryResult=await DB.AddUser(username,email,password);
         if (queryResult.affectedRows) {
            console.log("New user added!!")
          }
               
        }
        catch(err){
            console.log(err)
            res.sendStatus(500)
        }    
    }
    else
    {
        console.log("A field is missing!")
    }

    res.end();

    
});

module.exports=users
```

- Similarly to **novice.js** we import the required dependencies and modules.Then we write the logic for each route we define. In this case we want to consult: i) when user is trying to login and ii) when  he wants to register
- For sure you noticed that in both files **novice.js** and **users.js** we are using the custom methods we created in **dbConn.js**. Also that we are using special keywords, such as: await and async.For now the only thing you need to know about them is that they are connected with the previously introduced *Promises*

5. Save and run our server to check that everything works. 

- You can use tools like [Postman](https://www.postman.com/) to send request to your server. 

For now we have reached the end of our 1/3 tutorials. There are a few more thing to do in the back-end but we will finish them during the last tutorial. 

Lastly, once you finish the coding, zip you project and submit it to e-classroom.