# DevOpsTraining
**DevOps/Cloud Training Material**

# 1.4 MongoDB Database

We need a database where we will store our data. For this, we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), making it easier to manage our database.

## Sign Up for mLab

1. **Sign up for a shared clusters free account** [here](https://www.mongodb.com/cloud/atlas/register).
2. **Select AWS as the cloud provider** and choose a region near you.
3. **Complete the get started checklist**.

## Allow Access to the MongoDB Database

1. **Allow access from anywhere** (not secure, but ideal for testing).

   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/b9d2dc2c-9de3-4257-946f-379726b69629)

2. **IMPORTANT NOTE:** Change the deletion time from 6 Hours to 1 Week.

   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/0e91163f-536c-4c89-aa14-3a2f74509099)

## Create a MongoDB Database and Collection

1. **Create a new MongoDB database** and a collection inside mLab.

    ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/831770c2-b0f4-4cc9-b46c-ba47b7fb2c46)

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

    ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/c06dc8a3-56df-44b3-9bb3-45dd2d4e7cf0)

## Update `index.js` to Use `.env`

1. **Open the file with vim:**

    ```sh
    nano index.js
    ```

2. **Delete the existing content:**

3. **Paste the entire code below:**

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
    mongoose.connect(process.env.DB)
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
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/4ea0417a-218f-4a11-81a2-75184eab0911)

You should see a message `Database connected successfully`. If so, we have our backend configured.
