# DevOpsTraining
**DevOps/Cloud Training Material**

# 1.5 Testing Backend Code without Frontend using RESTful API

So far, we have written the backend part of our To-Do application and configured a database, but we do not have a frontend UI yet. We will need ReactJS code to achieve that. However, during development, we will need a way to test our code using RESTful API. Therefore, we will use an API development client to test our code.

In this project, we will use Postman to test our API. [Click here](https://www.postman.com/downloads/) to download and install Postman on your machine.

[Click here](https://learning.postman.com/docs/getting-started/introduction/) to learn how to perform CRUD operations on Postman.

You should test all the API endpoints and make sure they are working. For the endpoints that require a body, you should send JSON back with the necessary fields since it’s what we setup in our code.

## Testing with Postman

### Create a POST Request

Open Postman and create a POST request to the API:

```
http://<PublicIP-or-PublicDNS>:5000/api/todos
```

This request sends a new task to our To-Do list so the application can store it in the database.

> **Note:** Make sure you set the header key `Content-Type` as `application/json`.

**Example JSON body**:

```json
{
    "action": "Learn DevOps"
}
```

**Example Header**:

| Key           | Value             |
|---------------|-------------------|
| Content-Type  | application/json  |

### Create a GET Request

Create a GET request to your API:

```
http://<PublicIP-or-PublicDNS>:5000/api/todos
```

This request retrieves all existing records from our To-Do application (backend requests these records from the database and sends them back as a response to the GET request).

### Create a DELETE Request (Optional Task)

To delete a task, you need to send its ID as part of the DELETE request. Create a DELETE request to your API:

```
http://<PublicIP-or-PublicDNS>:5000/api/todos/<taskID>
```

Replace `<taskID>` with the actual ID of the task you want to delete.

**Example**:

```
http://<PublicIP-or-PublicDNS>:5000/api/todos/60d21b4667d0d8992e610c85
```

## Summary

By now you have tested the backend part of our To-Do application and have made sure that it supports all three operations we wanted:

- [x] **Display a list of tasks** - HTTP GET request
- [x] **Add a new task to the list** - HTTP POST request
- [x] **Delete an existing task from the list** - HTTP DELETE request

Using Postman, you can easily test and verify the functionality of your backend code without having a frontend UI in place.
