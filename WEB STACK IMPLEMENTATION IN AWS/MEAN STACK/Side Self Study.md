# DevOpsTraining
**DevOps/Cloud Training Material**

# MEAN Stack Components

The MEAN stack is a combination of the following components:

1. **MongoDB (Document database)**:
   - Stores and allows retrieval of data.
   - It is a NoSQL database, which means it stores data in a flexible, JSON-like format.

2. **Express (Back-end application framework)**:
   - Makes requests to the database for reads and writes.
   - It is a minimal and flexible Node.js web application framework that provides a robust set of features to develop web and mobile applications.

3. **Angular (Front-end application framework)**:
   - Handles client and server requests.
   - It is a platform and framework for building single-page client applications using HTML and TypeScript.

4. **Node.js (JavaScript runtime environment)**:
   - Accepts requests and displays results to the end user.
   - It is an open-source, cross-platform, JavaScript runtime environment that executes JavaScript code outside of a web browser.

# Side Self Study

## Refresh Your Knowledge of the OSI Model

The OSI (Open Systems Interconnection) model is a conceptual framework used to understand network interactions in seven layers. These layers are:

1. **Physical**: Hardware transmission technologies.
2. **Data Link**: Data transfer between adjacent network nodes.
3. **Network**: Routing of data packets.
4. **Transport**: Reliable data transfer.
5. **Session**: Inter-host communication.
6. **Presentation**: Data translation and encryption.
7. **Application**: Network services to end-users.

## Read About Load Balancing

Load balancing is a technique used to distribute network or application traffic across multiple servers. This ensures no single server becomes overwhelmed, thereby improving responsiveness and availability. 

### Types of Load Balancing:

1. **Hardware Load Balancers**: Physical devices that distribute traffic.
2. **Software Load Balancers**: Software applications that manage traffic distribution.
3. **DNS Load Balancing**: Uses DNS to distribute traffic based on the DNS request.

### Techniques of Load Balancing:

1. **Round Robin**: Each server is selected in turn.
2. **Least Connections**: Directs traffic to the server with the least number of active connections.
3. **IP Hash**: Routes traffic based on the client's IP address.
4. **Weighted Distribution**: Distributes traffic based on server capacity.

## Practice in Editing Simple Web Forms with HTML, CSS, and JS

- **HTML (Hypertext Markup Language)**: The standard language for creating web pages.
- **CSS (Cascading Style Sheets)**: Used to describe the presentation of a document written in HTML.
- **JavaScript (JS)**: A programming language that allows you to implement complex features on web pages.

### Example Practice:

1. **Create a Simple Web Form**:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Simple Form</title>
       <link rel="stylesheet" href="styles.css">
   </head>
   <body>
       <form id="simpleForm">
           <label for="name">Name:</label>
           <input type="text" id="name" name="name"><br><br>
           <label for="email">Email:</label>
           <input type="email" id="email" name="email"><br><br>
           <input type="submit" value="Submit">
       </form>
       <script src="script.js"></script>
   </body>
   </html>
   ```

2. **Style the Form with CSS**:
   ```css
   body {
       font-family: Arial, sans-serif;
       display: flex;
       justify-content: center;
       align-items: center;
       height: 100vh;
       background-color: #f0f0f0;
   }

   form {
       background: #fff;
       padding: 20px;
       border-radius: 5px;
       box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
   }

   label {
       margin-bottom: 10px;
       display: block;
   }

   input[type="text"],
   input[type="email"] {
       width: 100%;
       padding: 8px;
       margin-bottom: 10px;
       border: 1px solid #ccc;
       border-radius: 4px;
   }

   input[type="submit"] {
       background: #28a745;
       color: #fff;
       padding: 10px 20px;
       border: none;
       border-radius: 4px;
       cursor: pointer;
   }

   input[type="submit"]:hover {
       background: #218838;
   }
   ```

3. **Add JavaScript for Form Validation**:
   ```javascript
   document.getElementById('simpleForm').addEventListener('submit', function(event) {
       event.preventDefault();
       let name = document.getElementById('name').value;
       let email = document.getElementById('email').value;
       if(name === '' || email === '') {
           alert('Please fill in all fields');
       } else {
           alert(`Form submitted!\nName: ${name}\nEmail: ${email}`);
       }
   });
   ```

By refreshing your knowledge on the OSI model, understanding load balancing, and practicing with HTML, CSS, and JavaScript, you'll be well-prepared for building and maintaining web applications using the MEAN stack.
