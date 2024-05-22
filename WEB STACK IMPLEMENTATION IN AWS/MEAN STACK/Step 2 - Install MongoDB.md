# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 2: Install MongoDB

MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document, and the data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, ISBN number, author, and the number of pages.

## Add MongoDB Repository

First, add the MongoDB repository key:

```sh
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

Then, add the MongoDB repository to your sources list:

```sh
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

## Install MongoDB

Update the package list and install MongoDB:

```sh
sudo apt update
sudo apt install -y mongodb
```

## Start the MongoDB Server

Start the MongoDB service:

```sh
sudo service mongodb start
```

## Verify that the Service is Running

Check the status of the MongoDB service to ensure it is up and running:

```sh
sudo systemctl status mongodb
```

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

