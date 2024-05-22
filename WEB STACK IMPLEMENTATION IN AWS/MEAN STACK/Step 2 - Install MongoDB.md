# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 2: Install MongoDB

MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document, and the data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, ISBN number, author, and the number of pages.

## Add MongoDB Repository

First get the repository key of the latest version with the command:

```sh
curl -fsSL https://pgp.mongodb.com/server-7.0.asc |  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
```

Then, add the MongoDB repository to your sources list:

```sh
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

## Install MongoDB

Update the package list and install MongoDB:

```sh
sudo apt update
sudo apt install mongodb-org -y
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/f8afd54a-7471-4126-b46a-1437d47a535b)

## Start the MongoDB Server

Start the MongoDB service:

```sh
sudo systemctl start mongod
```

## Verify that the Service is Running

Check the status of the MongoDB service to ensure it is up and running:

```sh
sudo systemctl status mongod
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/f2c6f5f4-590f-49a9-a51b-f8e060bd4f0a)

## Install npm (Node Package Manager)



Install npm, which is the package manager for Node.js:

```sh
sudo apt install -y npm
```

## Install 'body-parser' Package

We need the 'body-parser' package to help us process JSON files passed in requests to the server. Install it using npm:

```sh
sudo npm install body-parser
```

## Set Up the Books Directory

Create a new directory named 'Books' and navigate into it:

```sh
mkdir Books && cd Books
```

## Initialize the npm Project

In the 'Books' directory, initialize a new npm project:

```sh
npm init
```

## Create the `server.js` File

Add a file named `server.js`:

```sh
vi server.js
```

Copy and paste the following web server code into the `server.js` file:

```javascript
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```

This sets up a basic Express server that listens on port 3300 and uses `body-parser` to handle JSON requests.

