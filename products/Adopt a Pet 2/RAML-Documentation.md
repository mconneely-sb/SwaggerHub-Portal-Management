 # Users Specification

A sample client implementation of the pet adoption workflow in typescript.


```typescript
// Assuming node environment. If using in a browser, fetch is globally available.

#%RAML 1.0
title: User API
description: RESTful API for managing users and their accounts
version: "1.0.0"
baseUri: https://virtserver.swaggerhub.com/MConneely/user-accounts-api/1.0.0
securitySchemes:
 bearerAuth:
   type: Pass Through
   describedBy:
     headers:
       Authorization:
         description: Bearer JWT token
         type: string
         pattern: "^Bearer .+$"
         example: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
types:
 User:
   description: A user object
   type: object
   properties:
     id:
       description: Unique identifier for the user
       type: integer
       required: false
     username:
       description: The user's username
       type: string
       required: false
     email:
       description: The user's email address
       type: string
       required: false
 Account:
   description: A user account object
   type: object
   properties:
     accountId:
       description: Unique identifier for the account
       type: integer
       required: false
     type:
       description: "The account type (e.g., savings, checking)"
       type: string
       required: false
     balance:
       description: The current balance of the account
       type: number
       format: float
       required: false
 CreateUserBody:
   type: object
   properties:
     username:
       description: The desired username for the new user
       type: string
       required: false
     email:
       description: The user's email address
       type: string
       required: false
     password:
       description: The user's password (should be stored securely)
       type: string
       required: false
 UpdateUserBody:
   type: object
   properties:
     username:
       description: The updated username for the user
       type: string
       required: false
     email:
       description: The updated email address for the user
       type: string
       required: false
 CreateAccountBody:
   type: object
   properties:
     type:
       description: "The type of account (e.g., savings, checking)"
       type: string
       required: false
     balance:
       description: Initial balance for the account
       type: number
       format: float
       required: false
 UpdateAccountBody:
   type: object
   properties:
     type:
       description: The updated account type
       type: string
       required: false
     balance:
       description: The updated account balance
       type: number
       format: float
       required: false
/users:
 securedBy: [bearerAuth]
 get:
   displayName: getAllUsers
   description: Returns a list of all users. Requires bearer authentication.
   responses:
     "200":
       description: A list of users
       body:
         application/json:
           type: User[]
           example:
             - id: 1
               username: johndoe
               email: johndoe@example.com
             - id: 2
               username: janedoe
               email: janedoe@example.com
 post:
   displayName: createUser
   description: "Create a new user with a username, email, and password. Requires bearer authentication."
   body:
     application/json:
       type: CreateUserBody
       example:
         username: newuser
         email: newuser@example.com
         password: strongpassword123
   responses:
     "201":
       description: User created successfully
       body:
         application/json:
           type: User
           example:
             id: 3
             username: newuser
             email: newuser@example.com
 /{userId}:
   uriParameters:
     userId:
       description: The unique identifier of the user
       type: integer
   get:
     displayName: getUserById
     description: Retrieve a single user by their unique ID. Requires bearer authentication.
     responses:
       "200":
         description: A single user
         body:
           application/json:
             type: User
             example:
               id: 1
               username: johndoe
               email: johndoe@example.com
       "404":
         description: User not found
   put:
     displayName: updateUser
     description: Update username and/or email for an existing user. Requires bearer authentication.
     body:
       application/json:
         type: UpdateUserBody
         example:
           username: johndoeupdated
           email: johndoeupdated@example.com
     responses:
       "200":
         description: User updated successfully
         body:
           application/json:
             type: User
             example:
               id: 1
               username: johndoeupdated
               email: johndoeupdated@example.com
       "404":
         description: User not found
   delete:
     displayName: deleteUser
     description: Delete a user by ID. Requires bearer authentication.
     responses:
       "204":
         description: User deleted successfully
       "404":
         description: User not found
   /accounts:
     get:
       displayName: getUserAccounts
       description: Get all accounts associated with a user. Requires bearer authentication.
       responses:
         "200":
           description: List of user accounts
           body:
             application/json:
               type: Account[]
               example:
                 - accountId: 101
                   type: savings
                   balance: 1500.75
                 - accountId: 102
                   type: checking
                   balance: 500
         "404":
           description: User not found
     post:
       displayName: createUserAccount
       description: "Create a new account (e.g., savings or checking) for the specified user. Requires bearer authentication."
       body:
         application/json:
           type: CreateAccountBody
           example:
             type: savings
             balance: 1000
       responses:
         "201":
           description: Account created successfully
           body:
             application/json:
               type: Account
               example:
                 accountId: 103
                 type: savings
                 balance: 1000
         "404":
           description: User not found
     /{accountId}:
       uriParameters:
         accountId:
           description: The unique identifier of the account
           type: integer
       get:
         displayName: getUserAccountById
         description: Retrieve details for a specific account belonging to a user. Requires bearer authentication.
         responses:
           "200":
             description: A single user account
             body:
               application/json:
                 type: Account
                 example:
                   accountId: 101
                   type: savings
                   balance: 1500.75
           "404":
             description: Account or user not found
       put:
         displayName: updateUserAccount
         description: Update the type or balance of a user's account. Requires bearer authentication.
         body:
           application/json:
             type: UpdateAccountBody
             example:
               type: checking
               balance: 2000
         responses:
           "200":
             description: Account updated successfully
           "404":
             description: Account or user not found
       delete:
         displayName: deleteUserAccount
         description: Delete a specific account belonging to a user. Requires bearer authentication.
         responses:
           "204":
             description: Account deleted successfully
           "404":
             description: Account or user not found

```

This RAML example is structured to execute. 