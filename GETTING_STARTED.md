```
mkdir transcendance && cd transcendance
```
Install the nest CLI globally
```
npm install -g @nestjs/cli
```
BACKEND
=======

```
nest new backend && cd backend
```
Prisma
------
Prisma generates SQL tables for `.prisma` files
Install prisma as dev tool
```
npm install prisma --save-dev
```
Create a prisma directory with a schema file and a .env file where the database
URL must be specified
```
npx prisma init
```
Now create a schema file and then generate t
