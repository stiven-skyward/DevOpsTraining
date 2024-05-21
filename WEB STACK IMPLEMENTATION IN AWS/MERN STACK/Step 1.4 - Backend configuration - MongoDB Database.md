# DevOpsTraining
**DevOps/Cloud Training Material**

# MongoDB Database

We need a database where we will store our data. For this, we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), making it easier to manage our database.

## Sign Up for mLab

1. **Sign up for a shared clusters free account** [here](https://www.mongodb.com/cloud/atlas/register).
2. **Select AWS as the cloud provider** and choose a region near you.
3. **Complete the get started checklist** as shown in the image below.

![Get Started Checklist](path/to/image.png)

## Allow Access to the MongoDB Database

1. **Allow access from anywhere** (not secure, but ideal for testing).

    ![Allow Access](path/to/image.png)

2. **IMPORTANT NOTE:** Change the deletion time from 6 Hours to 1 Week.

## Create a MongoDB Database and Collection

1. **Create a new MongoDB database** and a collection inside mLab.

    ![Create Database](path/to/image.png)

## Environment Variables

In the `index.js` file, we specified `process.env` to access environment variables, but we have not yet created this file. So we need to do that now.

1. **Create a file in your Todo directory and name it `.env`.**

    ```sh
    touch .env
    nano .env
    ```

2. **Add the connection string to access the database in it, just as below:**

    ```plaintext
    DB='mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
    ```

    Ensure to update `<username>`, `<password>`, `<network-address>`, and `<dbname>` according to your setup.

    ![Get Connection String](path/to/image.png)

## Update `index.js` to Use `.env`

1. **Open the file with vim:**

    ```sh
    vim index.js
    ```

2. **Delete the existing content:**

    - Press `esc`
    - Type `:`
    - Type `%d`
    - Hit `Enter`

    The entire content will be deleted.

3. **Enter insert mode and paste the entire code below:**

    - Press `i` to enter insert mode.
    - Paste the following code:

    ```javascript
    const express = require('express');
    const bodyParser = require('body-parser');
    const mongoose = require('mongoose');
    const routes = require('./routes/api');
    const path = require('path');
    require('dotenv').config();

    const app = express();

    const port = process.env.PORT || 5000;

    // Connect to the database
    mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
        .then(() => console.log(`Database connected successfully`))
        .catch(err => console.log(err));

    // Override mongoose promise with node's promise
    mongoose.Promise = global.Promise;

    app.use((req, res, next) => {
        res.header("Access-Control-Allow-Origin", "*");
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
        console.log(`Server running on port ${port}`);
    });
    ```

Using environment variables to store information is considered more secure and a best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the `index.js` application file.

## Start Your Server

Start your server using the command:

```sh
node index.js
```

You should see a message `Database connected successfully`. If so, we have our backend configured.
