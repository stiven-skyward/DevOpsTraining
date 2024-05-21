# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 1.3 - Models

Now comes the interesting part. Since the app is going to make use of MongoDB, which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript-based applications, and it is what makes them interactive.

We will also use models to define the database schema. This is important so that we can define the fields stored in each MongoDB document. This may seem like a lot of information, but don't worry, everything will become clear to you over time. I promise!

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties.

## Install Mongoose

To create a Schema and a model, install Mongoose, which is a Node.js package that makes working with MongoDB easier.

Change directory back to the Todo folder:

```sh
cd ..
```

Install Mongoose:

```sh
npm install mongoose
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/79ddd9e0-cc9e-4f10-9678-ffe7982d9969)

## Create the Models Directory and File

Create a new folder for models:

```sh
mkdir models
```

Change directory into the newly created `models` folder:

```sh
cd models
```

Inside the `models` folder, create a file named `todo.js`:

```sh
touch todo.js
```

> **Tip:** All three commands above can be defined in one line to be executed consecutively with the help of the `&&` operator, like this:
>
> ```sh
> mkdir models && cd models && touch todo.js
> ```

Open the file created with `nano todo.js`, then paste the code below into the file:

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

Save and exit the file.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/37a57db0-4058-4bec-b5d3-c6117881e579)

## Update Routes to Use the New Model

We need to update our routes in the `api.js` file in the `routes` directory to make use of the new model.

Change directory to the `routes` directory:

```sh
cd ../routes
```

Open `api.js` with `nano api.js`, delete the existing code, and paste the code below into it:

```javascript
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {
    // This will return all the data, exposing only the id and action field to the client
    Todo.find({}, 'action')
        .then(data => res.json(data))
        .catch(next);
});

router.post('/todos', (req, res, next) => {
    if (req.body.action) {
        Todo.create(req.body)
            .then(data => res.json(data))
            .catch(next);
    } else {
        res.json({
            error: "The input field is empty"
        });
    }
});

router.delete('/todos/:id', (req, res, next) => {
    Todo.findOneAndDelete({ "_id": req.params.id })
        .then(data => res.json(data))
        .catch(next);
});

module.exports = router;
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/c31892f7-324d-4839-b7f7-158cabc326a9)

Save and exit the file.
