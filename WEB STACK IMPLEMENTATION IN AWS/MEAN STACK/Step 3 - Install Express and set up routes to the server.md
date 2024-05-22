# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 3: Install Express and Set Up Routes to the Server

Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express to pass book information to and from our MongoDB database.

We will also use the Mongoose package, which provides a straightforward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.

## Install Express and Mongoose

First, install the required packages:

```sh
sudo npm install express mongoose
```

## Set Up the Application Structure

1. **Create the `apps` folder** in the `Books` directory:

    ```sh
    mkdir apps && cd apps
    ```

2. **Create a file named `routes.js`**:

    ```sh
    vi routes.js
    ```

3. **Copy and paste the following code into `routes.js`**:

    ```javascript
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 

      app.post('/book', function(req, res) {
        var book = new Book({
          name: req.body.name,
          isbn: req.body.isbn,
          author: req.body.author,
          pages: req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json({
            message: "Successfully added book",
            book: result
          });
        });
      });

      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json({
            message: "Successfully deleted the book",
            book: result
          });
        });
      });

      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

## Set Up the Book Model

1. **In the `apps` folder, create a folder named `models`**:

    ```sh
    mkdir models && cd models
    ```

2. **Create a file named `book.js`**:

    ```sh
    vi book.js
    ```

3. **Copy and paste the following code into `book.js`**:

    ```javascript
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);

    var bookSchema = mongoose.Schema({
      name: String,
      isbn: { type: String, index: true },
      author: String,
      pages: Number
    });

    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema);
    ```

By following these steps, you will have set up Express and configured the routes to handle CRUD operations for your book register. You will also have established a Mongoose schema to interact with your MongoDB database.
