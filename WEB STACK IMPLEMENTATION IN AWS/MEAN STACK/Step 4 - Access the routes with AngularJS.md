# DevOpsTraining
**DevOps/Cloud Training Material**

# Step 4: Access the Routes with AngularJS

AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.

## Setting Up AngularJS

1. **Change the directory back to `Books`**:

    ```sh
    cd ../..
    ```

2. **Create a folder named `public`**:

    ```sh
    mkdir public && cd public
    ```

3. **Add a file named `script.js`**:

    ```sh
    nano script.js
    ```

4. **Copy and paste the following code into `script.js`**:

    ```javascript
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http({
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });

      $scope.del_book = function(book) {
        $http({
          method: 'DELETE',
          url: '/book/:isbn',
          params: { 'isbn': book.isbn }
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };

      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```

5. **In the `public` folder, create a file named `index.html`**:

    ```sh
    nano index.html
    ```

6. **Copy and paste the following code into `index.html`**:

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td>
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td>
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

## Start the Server

1. **Change the directory back up to `Books`**:

    ```sh
    cd ..
    ```

2. **Start the server** by running this command:

    ```sh
    node server.js
    ```

The server is now up and running, and we can connect to it via port 3300. You can launch a separate Putty or SSH console to test what the `curl` command returns locally.

```sh
curl -s http://localhost:3300
```
![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/05720138-79d1-4a17-821e-2a0ac6cb7ea3)

It should return an HTML page. Although it may be hard to read in the CLI, you can also try and access it from the Internet.

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/1ec264c2-50ba-43a6-9561-cea2931f7ef0)

## Open TCP Port 3300 in AWS

To access the application from the Internet, you need to open TCP port 3300 in your AWS Web Console for your EC2 Instance.

You should already know how to do this. If you have forgotten, refer to Project 1 (Step 1 — Installing Apache and Updating the Firewall).

Your Security group should look like this:

![image](https://github.com/stiven-skyward/DevOpsTraining/assets/135337796/8c620bc0-e483-4013-8dfe-4b9bd6a0bf39)

Now you can access our Book Register web application from the Internet with a browser using the Public IP address or Public DNS name.

### Quick Reminder to Get Your Server's Public IP and Public DNS Name:

- You can find it in your AWS web console in EC2 details.
- Run `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` for the Public IP address or `curl -s http://169.254.169.254/latest/meta-data/public-hostname` for the Public DNS name.

By following these steps, you will set up AngularJS to interact with your Express routes and MongoDB database, enabling a full-stack web application.
