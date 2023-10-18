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

    Authorization: "Bearer {{token}}"

### Get user's contacts success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: [
    {
        "_id": "exampleid",
        "name": "examplename",
        "email": "example@example.com",
        "phone": "examplephone",
        "favorite": false,
        "owner": {
            "_id": "exampleid",
            "email": "example@example.com"
        }
    }
]

### Get user's contacts unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Get contact by id

`GET /api/contacts/:id`

    Authorization: "Bearer {{token}}"

### Get contact by id success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "_id": "exampleid",
        "name": "example name",
        "email": "example@example.com",
        "phone": "examplephone",
        "favorite": true,
        "owner": "exampleid",
        "createdAt": "exampledate",
        "updatedAt": "exampledate"
}

### Get contact by id not found

    Status: 404 Not Found
    Content-Type: application/json
    ResponseBody: {
        "message": "Not found"
    }

### Get contact by id wrong id format

    Status: 400 Bad request
    Content-Type: application/json
    ResponseBody: {
        "message": "<Id> is not valid id"
    }

### Get contact by id unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Add new contact

`POST /api/contacts`

    Authorization: "Bearer {{token}}"
    Content-Type: application/json
    RequestBody: {
        "name": "examplename",
        "email": "example@example.com",
        "phone": "examplephone",
    }

### Add new contact success response

    Status: 201 Created
    Content-Type: application/json
    ResponseBody: {
        "name": "examplename",
        "email": "example@example.com",
        "phone": "examplephone",
        "favorite": false,
        "owner": "exampleid",
        "_id": "exampleid",
        "createdAt": "exampledate",
        "updatedAt": "exampledate"
    }

### Add new contact missing required fields error

    Status: 400 Bad Request
    Content-Type: application/json
    ResponseBody: {
        "message": "missing required <field name> field"
    }

### Add new contact unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Delete contact

`DELETE /api/contacts/:id`

    Authorization: "Bearer {{token}}"

### Delete contact success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "message": "Contact deleted"
    }

### Contact not found response

    Status: 404 Not Found
    Content-Type: application/json
    ResponseBody: {
        "message": "Not found"
    }

### Delete contact unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    }

## Update contact

`PUT /api/contacts/:id`

    Authorization: "Bearer {{token}}"
    Content-Type: application/json
    RequestBody: {
        "name": "examplename",
        "email": "example@example.com",
        "phone": "examplephone",
    }

### Update contact success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "_id": "exampleid",
        "name": "example name",
        "email": "example@example.com",
        "phone": "examplephone",
        "favorite": true,
        "owner": "exampleid",
        "createdAt": "exampledate",
        "updatedAt": "exampledate"
    }

### Update contact missing required fields error

    Status: 400 Bad Request
    Content-Type: application/json
    ResponseBody: {
        "message": "missing required <field name> field"
    }

### Update contact unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    } 

### Contact not found response

    Status: 404 Not Found
    Content-Type: application/json
    ResponseBody: {
        "message": "Not found"
    }

## Update favorite field

`PATCH / api / contacts /: contactId / favorite`

    Authorization: "Bearer {{token}}"
    Content-Type: application/json
    RequestBody: {
        "favorite": "true" | "false"
    }

### Update favorite field success response

    Status: 200 OK
    Content-Type: application/json
    ResponseBody: {
        "_id": "exampleid",
        "name": "example name",
        "email": "example@example.com",
        "phone": "examplephone",
        "favorite": true,
        "owner": "exampleid",
        "createdAt": "exampledate",
        "updatedAt": "exampledate"
    }

### Update favorite missing required fields error

    Status: 400 Bad Request
    Content-Type: application/json
    ResponseBody: {
        "message": "missing field favorite"
    }

### Update favorite field unauthorized error

    Status: 401 Unauthorized
    Content-Type: application/json
    ResponseBody: {
        "message": "Not authorized"
    } 

### Contact not found response

    Status: 404 Not Found
    Content-Type: application/json
    ResponseBody: {
        "message": "Not found"
    }