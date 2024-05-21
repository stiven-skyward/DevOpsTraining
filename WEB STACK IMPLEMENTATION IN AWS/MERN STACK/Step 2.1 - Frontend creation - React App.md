# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 2 - Frontend Creation

Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the `create-react-app` command to scaffold our app.

## Setting Up the React App

1. **Create React App**:
   In the same root directory as your backend code, which is the Todo directory, run:
   
   ```sh
   npm install -g yarn
   yarn create react-app client
   
   ```
   
   This will create a new folder in your Todo directory called `client`, where you will add all the React code.

   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/0cdd2e92-bf6c-41d5-9098-0a6b7a62c1fa)

## Running a React App

Before testing the React app, there are some dependencies that need to be installed.

1. **Install concurrently**:
   It is used to run more than one command simultaneously from the same terminal window.
   
   ```sh
   npm install concurrently --save-dev
   ```
   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/94e97685-b412-456b-886b-d963bc549b36)

2. **Install nodemon**:
   It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
   
   ```sh
   npm install nodemon --save-dev
   ```
   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/8e7ef021-5cc0-49ed-bca0-6b488eab1484)

3. **Update package.json**:
   In the Todo folder, open the `package.json` file. Change the `scripts` section and replace it with the code below:
   
   ```json
   "scripts": {
       "start": "node index.js",
       "start-watch": "nodemon index.js",
       "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
   },
   ```

4. **Configure Proxy in package.json**:
   Change directory to `client`:
   
   ```sh
   cd client
   ```

   Open the `package.json` file:
   
   ```sh
   nano package.json
   ```

   Add the key-value pair in the `package.json` file:
   
   ```json
   "proxy": "http://localhost:5000"
   ```

   The whole purpose of adding the proxy configuration above is to make it possible to access the application directly from the browser by simply calling the server URL like `http://localhost:5000` rather than always including the entire path like `http://localhost:5000/api/todos`.

5. **Run the App**:
   Now, ensure you are inside the Todo directory, and simply run:
   
   ```sh
   npm run dev
   ```

   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/3dd614f8-7c1e-411d-b9e5-251bc464c996)

   Your app should open and start running on `http://localhost:3000`.

   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/1fd06a01-aa84-4741-83b0-e4891d1c34b0)

> **Important note**: In order to be able to access the application from the Internet, you have to open TCP port 3000 on EC2 by adding a new Security Group rule. You already know how to do it.

   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/41e93df4-af6c-4c25-bd7a-206153b61447)

By following these steps, you will have your frontend set up and running, ready to interact with your backend API.
