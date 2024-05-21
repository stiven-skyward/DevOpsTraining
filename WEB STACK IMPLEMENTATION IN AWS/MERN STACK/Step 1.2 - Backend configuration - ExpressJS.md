# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 1.2 - Installing ExpressJS

Express is a framework for Node.js that simplifies development by abstracting a lot of low-level details. For example, Express helps define routes of your application based on HTTP methods and URLs.

## Install Express

To use Express, install it using npm:

```sh
npm install express
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/f4dba715-db97-47ee-a111-fe341f6b686f)

## Create `index.js`

Now create a file `index.js` with the command below:

```sh
touch index.js
```

Run `ls` to confirm that your `index.js` file is successfully created.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/c654235d-5be6-4939-b15d-86a01a44fe92)

## Install dotenv Module

Install the `dotenv` module to manage environment variables:

```sh
npm install dotenv
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/3231dc6d-9359-4944-8977-2a0b54e60818)

## Edit `index.js`

Open the `index.js` file with the command below:

```sh
nano index.js
```

Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.

```javascript
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
    console.log(`Server running on port ${port}`)
});
```

Notice that we have specified to use port `5000` in the code. This will be required later when we go on the browser.

Use `Ctrl`+`O` and press `ENTER` to save in nano and use `Ctrl`+`X` to exit nano.

## Start the Server

Now it is time to start our server to see if it works. Open your terminal in the same directory as your `index.js` file and type:

```sh
node index.js
```

If everything goes well, you should see `Server running on port 5000` in your terminal.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/b1affde8-e667-4e1a-8cec-bee636f252a4)

## Open Port 5000 in EC2 Security Groups

Now we need to open this port in EC2 Security Groups. Refer to **Project 1 Step 1 – Installing the Nginx Web Server**. There we created an inbound rule to open TCP port 80; you need to do the same for port 5000.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/126941f3-5391-4e65-9694-185e690fc88e)

## Access the Server

Open up your browser and try to access your server's Public IP or Public DNS name followed by port 5000:

```sh
http://<PublicIP-or-PublicDNS>:5000
```

### Quick Reminder: How to Get Your Server's Public IP and Public DNS Name

- You can find it in your AWS web console in EC2 details.
- Run `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` for Public IP address or `curl -s http://169.254.169.254/latest/meta-data/public-hostname` for Public DNS name.

You should see `Welcome to Express` in your browser.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/4019ab14-7ba9-4bac-bdfc-ee72b27cc61f)

## Routes

There are three actions that our To-Do application needs to be able to do:
- Create a new task
- Display list of all tasks
- Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: `POST`, `GET`, `DELETE`.

For each task, we need to create routes that will define various endpoints that the To-Do app will depend on.

## Create Routes Directory

Let's create a folder `routes`:

```sh
mkdir routes
```

> **Tip:** You can open multiple shells in Putty or Linux/Mac to connect to the same EC2 instance.

Change directory to the `routes` folder:

```sh
cd routes
```

Now, create a file `api.js` with the command below:

```sh
touch api.js
```

Open the file with the command below:

```sh
vim api.js
```

Copy the code below into the file. (Do not be overwhelmed with the code):

```javascript
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {
    // Code to get all todos
});

router.post('/todos', (req, res, next) => {
    // Code to create a new todo
});

router.delete('/todos/:id', (req, res, next) => {
    // Code to delete a todo
});

module.exports = router;
```

Save and exit the file.
