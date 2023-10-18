# REST API for Contacts App

This REST API is deployed at the following address: https://contacts-app-backend-kwn6.onrender.com

## USER REQUESTS

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
    ResponseBody: <Error from Joi or other validation library>

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
    ResponseBody: <Error from Joi or other validation library>

## Get current user

`GET /api/users/current`

    Authorization: "Bearer {{token}}"

### Current user success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "email": "example@example.com",
        "subscription": "starter"
    }

### Current user unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Logout

`POST /api/users/logout`

    Authorization: "Bearer {{token}}"

### Logout success response

    Status: 204 No Content

### Logout unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Subscription type update

`PATCH /api/users`

    Authorization: "Bearer {{token}}"
    Content-Type: application/json
    RequestBody: {
        "subscription": "starter" | "pro" | "business"
    }

### Subscription update success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "_id": "exampleid",
        "email": "example@example.com",
        "password": "examplepassword",
        "subscription": "pro",
        "token": "exampletoken",
        "avatarUrl": "//www.gravatar.com/avatar/5ed181b6895aea1a0cca372abd5c3759",
        "verified": true,
        "verificationToken": null,
        "createdAt": "exampledate",
        "updatedAt": "exampledate"

}

### Subscription update unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Avatar update

`PATCH /api/users/avatars`

    Authorization: "Bearer {{token}}"
    Content-Type: multipart/form-data
    RequestBody: <uploaded file>

### Avatar update success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "avatarURL": <link to the image>
    }

### Avatar update unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Verification request

`GET /api/auth/verify/:verificationToken`

### Verification success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "message": "Verification successful"
    }

### Verification user not found

    Status: 404 Not Found
    Content-Type: application/json
    ResponseBody: {
        "message": "User not found"
    }

## Resending an email request

`POST /users/verify`

    Content-Type: application/json
    RequestBody: {
        "email": "example@example.com"
    }

### Resending an email success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "message": "Verification email sent"
    }

### Resending an email for verified user

    Status: 400 Bad Request
    Content-Type: application/json
    ResponseBody: {
        "message": "Verification has already been passed"
    }

### Resending an email validation error

    Status: 400 Bad Request
    Content-Type: application/json
    ResponseBody: {
        "message": <Error from Joi or other validation library>
    }

## CONTACTS REQUESTS

## Get user's contacts

`GET /api/contacts`

### Get user's contacts success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "user": {
        "email": "example@example.com",
        "subscription": "starter"
        }
    }