# REST API for Contacts App

This REST API is deployed at the following address: https://contacts-app-backend-kwn6.onrender.com

## AUTHENTICATION REQUESTS

## Registration

`POST /api/users/register`

    Content-Type: application/json
    RequestBody: {
        "email": "example@example.com",
        "password": "examplepassword"
    }

### Registration success response

    Status: 201 Created
    Content-Type: application/json
    ResponseBody: {
        "user": {
        "email": "example@example.com",
        "subscription": "starter"
        }
    }

 ### Registration conflict error

    Status: 409 Conflict
    Content-Type: application/json
    ResponseBody: {
        "message": "Email in use"
    }   

 ### Registration validation error

    Status: 400 Bad Request
    Content-Type: application/json
    ResponseBody: <Помилка від Joi або іншої бібліотеки валідації>

## Login

`POST /api/users/login`

    Content-Type: application/json
    RequestBody: {
        "email": "example@example.com",
        "password": "examplepassword"
    }

### Login success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "token": "exampletoken",
        "user": {
            "email": "example@example.com",
            "subscription": "starter"
        }
    }

 ### Login auth error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Email or password is wrong"
    }   

 ### Login validation error

    Status: 400 Bad Request
    Content-Type: application/json
    ResponseBody: <Помилка від Joi або іншої бібліотеки валідації>


## Get current user


