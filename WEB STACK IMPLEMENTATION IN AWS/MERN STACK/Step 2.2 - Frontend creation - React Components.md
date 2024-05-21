# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 2.2 - Creating Your React Components

One of the advantages of React is that it makes use of components, which are reusable and also make the code modular. For our Todo app, there will be two stateful components and one stateless component. 

## Setting Up the Components

1. **Navigate to the `client` directory**:
   
   ```sh
   cd client
   ```

2. **Move to the `src` directory**:

   ```sh
   cd src
   ```

3. **Create a `components` folder** inside the `src` folder:

   ```sh
   mkdir components
   ```

4. **Move into the `components` directory**:

   ```sh
   cd components
   ```

5. **Create the component files**:

   ```sh
   touch Input.js ListTodo.js Todo.js
   ```

### Creating the `Input` Component

1. **Open the `Input.js` file**:

   ```sh
   nano Input.js
   ```

2. **Copy and paste the following code into `Input.js`**:

   ```javascript
   import React, { Component } from 'react';
   import axios from 'axios';

   class Input extends Component {
       state = {
           action: ""
       }

       addTodo = () => {
           const task = { action: this.state.action }

           if(task.action && task.action.length > 0){
               axios.post('/api/todos', task)
                   .then(res => {
                       if(res.data){
                           this.props.getTodos();
                           this.setState({ action: "" });
                       }
                   })
                   .catch(err => console.log(err));
           } else {
               console.log('input field required');
           }
       }

       handleChange = (e) => {
           this.setState({ action: e.target.value });
       }

       render() {
           let { action } = this.state;
           return (
               <div>
                   <input type="text" onChange={this.handleChange} value={action} />
                   <button onClick={this.addTodo}>add todo</button>
               </div>
           );
       }
   }

   export default Input;
   ```

3. **Install Axios**:

   ```sh
   cd ../..
   npm install axios
   ```
   
   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/3837a5e5-e16c-47a7-8274-b510d9ae20bc)

### Creating the `ListTodo` Component

1. **Open the `ListTodo.js` file**:

   ```sh
   nano ListTodo.js
   ```

2. **Copy and paste the following code into `ListTodo.js`**:

   ```javascript
   import React from 'react';

   const ListTodo = ({ todos, deleteTodo }) => {
       return (
           <ul>
               {todos && todos.length > 0 ? (
                   todos.map(todo => (
                       <li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
                   ))
               ) : (
                   <li>No todo(s) left</li>
               )}
           </ul>
       );
   }

   export default ListTodo;
   ```

### Creating the `Todo` Component

1. **Open the `Todo.js` file**:

   ```sh
   nano Todo.js
   ```

2. **Copy and paste the following code into `Todo.js`**:

   ```javascript
   import React, { Component } from 'react';
   import axios from 'axios';

   import Input from './Input';
   import ListTodo from './ListTodo';

   class Todo extends Component {
       state = {
           todos: []
       }

       componentDidMount() {
           this.getTodos();
       }

       getTodos = () => {
           axios.get('/api/todos')
               .then(res => {
                   if(res.data) {
                       this.setState({ todos: res.data });
                   }
               })
               .catch(err => console.log(err));
       }

       deleteTodo = (id) => {
           axios.delete(`/api/todos/${id}`)
               .then(res => {
                   if(res.data) {
                       this.getTodos();
                   }
               })
               .catch(err => console.log(err));
       }

       render() {
           let { todos } = this.state;
           return (
               <div>
                   <h1>My Todo(s)</h1>
                   <Input getTodos={this.getTodos} />
                   <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
               </div>
           );
       }
   }

   export default Todo;
   ```

### Adjusting the `App` Component

1. **Move to the `src` directory**:

   ```sh
   cd ..
   ```

2. **Open the `App.js` file**:

   ```sh
   nano App.js
   ```

3. **Copy and paste the following code into `App.js`**:

   ```javascript
   import React from 'react';
   import Todo from './components/Todo';
   import './App.css';

   const App = () => {
       return (
           <div className="App">
               <Todo />
           </div>
       );
   }

   export default App;
   ```

### Adding CSS for the App

1. **Open the `App.css` file**:

   ```sh
   nano App.css
   ```

2. **Copy and paste the following code into `App.css`**:

   ```css
   .App {
       text-align: center;
       font-size: calc(10px + 2vmin);
       width: 60%;
       margin-left: auto;
       margin-right: auto;
   }

   input {
       height: 40px;
       width: 50%;
       border: none;
       border-bottom: 2px #101113 solid;
       background: none;
       font-size: 1.5rem;
       color: #787a80;
   }

   input:focus {
       outline: none;
   }

   button {
       width: 25%;
       height: 45px;
       border: none;
       margin-left: 10px;
       font-size: 25px;
       background: #101113;
       border-radius: 5px;
       color: #787a80;
       cursor: pointer;
   }

   button:focus {
       outline: none;
   }

   ul {
       list-style: none;
       text-align: left;
       padding: 15px;
       background: #171a1f;
       border-radius: 5px;
   }

   li {
       padding: 15px;
       font-size: 1.5rem;
       margin-bottom: 15px;
       background: #282c34;
       border-radius: 5px;
       overflow-wrap: break-word;
       cursor: pointer;
   }

   @media only screen and (min-width: 300px) {
       .App {
           width: 80%;
       }

       input {
           width: 100%;
       }

       button {
           width: 100%;
           margin-top: 15px;
           margin-left: 0;
       }
   }

   @media only screen and (min-width: 640px) {
       .App {
           width: 60%;
       }

       input {
           width: 50%;
       }

       button {
           width: 30%;
           margin-left: 10px;
           margin-top: 0;
       }
   }
   ```

3. **Open the `index.css` file**:

   ```sh
   nano index.css
   ```

4. **Copy and paste the following code into `index.css`**:

   ```css
   body {
       margin: 0;
       padding: 0;
       font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
       "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
       sans-serif;
       -webkit-font-smoothing: antialiased;
       -moz-osx-font-smoothing: grayscale;
       box-sizing: border-box;
       background-color: #282c34;
       color: #787a80;
   }

   code {
       font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
       monospace;
   }
   ```

### Running the App

1. **Move back to the Todo directory**:

   ```sh
   cd ../..
   ```

2. **Run the app**:

   ```sh
   npm run dev
   ```

   ![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/aefecfab-a105-4480-b1f4-03ff90b4de1c)

Assuming there are no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task, and viewing all your tasks.
