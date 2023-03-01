#TRANSCENDANCE

1st container : PostgreSQL docker image
2nd container: a node docker image for the backend
3rd container: for fronted, also a node image ?

```
mkdir transcendance && cd transcendance
```
Install the nest CLI globally
```
npm install -g @nestjs/cli
```
##BACKEND

```
nest new backend && cd backend
```
###Interfacing with the Database: Prisma

Prisma generates SQL tables for `.prisma` files
Install prisma as dev tool. Once the tables are generated, you won't need it.
```
npm install prisma --save-dev
```
This creates a prisma directory with a schema file and a .env file where the database
URL must be specified
```
npx prisma init
```
Specify the `DATABASE_URL=` in the `.env` file according to (this page)[https://www.prisma.io/docs/concepts/database-connectors/postgresql]

The default generated `schema.prisma` file is already configured for PostGreSQL

Now create your prisma `model`s and then generate the SQL tables and run them in
the database with :
```
npx prisma migrate dev --name init
```
It seems like the previous command does a lot and I can't break it down yet

You also need a prisma _client_ in order to make database _queries_ directly 
from your code without having to send SQL to the DB yourself
```
npm install @prisma/client
```
It's best to wrap the prisma client in a Nest service and then inject it in services 
that needs to interact with the database.
[Here](https://docs.nestjs.com/recipes/prisma#use-prisma-client-in-your-nestjs-services) is an example of how to do that.

Don't forget to add to prevent the prisma client to call process.exit on receiving a shudown signal as it
will prevent our shutdown hooks (do we need any ?) to be executed :
[](https://docs.nestjs.com/recipes/prisma#issues-with-enableshutdownhooks)

Edit: Actually one one side you need to generate the SQL_TABLE and optionnally run them against your db
but you also need to generate the prisma clien with prisma generate
Clarify later

### Creating modules and their services

We're going to need modules for :
- [Authentication](https://docs.nestjs.com/security/authentication#authentication)
- Game
- Users
- ? What else

```
nest g module auth game
```
Let's also create the prisma service in the `src` directory as it is likely to be injected in our
other services:
```
nest g service prisma-client-service
```
### Implement authenntication with the 42 api

[42 API docs](https://api.intra.42.fr/apidoc)
[NestJs Authentication](https://docs.nestjs.com/security/authentication#authentication)

Nest relies on the Passport library so:
```
npm install --save @nestjs/passport passport passport-jwt
npm install --save-dev @types/passport-jwt
```
Now let's `cd` into our `auth` and `nest generate service auth`

Apparently it is useful to also have a `user` service in its own `user` module. 
Use the `--flat` option when you don't want to generate a directory for the service you are creating

Also if you're gonna use the UsersService in the auth Module, you'd better export it at the in the user module.

### Install Axios
Nest's HttpModule is a wrapper around the axios library. Install both the wrapper and the library
```
npm i --save @nestjs/axios axios
```

## FRONTEND
