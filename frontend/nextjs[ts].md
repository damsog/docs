# Nextjs Guide

This repository contains personal guides for Backend Web Application development.

### I. DESIGN

The first thing to have in mind it's to know what you want to build, functionalities, options, operations etc, at least the core of the application because based on this application sketch we have to design our database.

### II. DEVELOPMENT

#### Creation and Configuration
Initialize the application. run the nextjs script to create the application template. follow the steps to add typescript, and use the experimental app directory

```sh
npm create-next-app@latest <app-name>
```

Noe, lets add Tailwind CSS.
```sh
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
This will create the tailwind configuration files, but we need to configure a couple of things. <br>
Edit the file ```tailwind.config.js``` and add the following paths
```json
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx}", // Note the addition of the `app` directory.
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",

    // Or if using `src` directory:
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
These are the routes for our views, in case you have these folders in ```src``` update the paths accordingly.<br>
Now we need to import the global styles. Go to ```app/globals.css``` or create it if doesn't exist, and add the following:
Edit the package.json and add the following scripts
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
Lastly, make sure that ```app/layout.tsx``` is importing these styles:
```tsx
// These styles apply to every route in the application
import './globals.css';
...
```
#### Folder Structure

```
<project-name>
|
+-- .base.env           Template. Set up String constants and credentials
+-- .env                Set up String constants and credentials
+-- prisma/             SQl Definition & ORM config (BE)
+-- public/             Public Resources and frontend goes here
+-- src/
    |
    +--app/
        |
        +-- api/        API Routes. for Integrated Backend (BE)
        +-- <...pages>  Pages and Views
    +-- controllers/    Endpoints and functions controllers (BE)
    +-- services/       Functions implementations (Controller implements) (BE)
    +-- models/         Data Models 
    +-- lib/            Personal Libs and helper functions
    +-- resources/      Backend Resources and info that doesnt go on the DB (BE)
+-- <...NextJs folders>
```

The script will create the base folder and most of the resources, just create the missing folder that will be used later. (Ignore BE, Backend folder if not working for a fullstack application)
This structure has in mind to separate the database design models, the services, and the controllers (routes) for separation of concerns and keep the project organized.

#### Environment Variables
Create a file to set the environment variables (global variables) for our application. <br>
create a base env file to add to the version control with a configuration template, but don't set sensitive information here, just put some generic description to the variables, and create another file called .env which will be added to .gitignore to not be saved to the version control and put your configutation there.<br>
This way, anyone who wants to set the server should copy the base env file and set its own variables.
```sh
touch .base.env
```
and set the env variables template
```
VARIABLE=<set-x>
VARIABLE2=<set-y>
```
and everytime a server is configured, just do
```
cp .base.env .env
```
And set the variables.

#### Version Control
Initialize a git repository on the project root to track any changes.
```sh
git init
```
Then create a .gitignore and describe files to NOT keep track of, like large files, sensitive data, like credentials, configuration files like .env (keep track of a base template that should not change much, .base.env but add .env to gitignore, because this file may change a lot and may contain sensitive info).<br>
<br>
For a node/express server some usual files to ignore are.
```
node_modules/
*.log
mydatabase_backup.sql
nohup.out
.env
```

#### Database
We may need an ORM to manage the DB. Sequelize its a popular ORM for Node.js. however, prisma is a newer alternative which is way better IMO.To use prisma start with
```sh
prisma init
```
this creates a .env file if it doesnt exist or adds the db url to it. also creates a prisma/schema.prisma file where the database is designed. <br>
follow this [guide]() to understand prisma's way of schema design<br>
After the DB is designed, you can run  migrations, which synchronizes the schema with the RDBMS.
```sh
npx prisma migrate dev --name <migration-name>
```

Then, generate the client bindings (to use the database models in code) with
```sh
npx prisma generate
```
Another cool feature of prisma is that you can run a server which generates a web client to check the database, using:
```sh
npx prisma studio
```

### Packages

#### Logging
[Winston](https://www.npmjs.com/package/winston) for logging

Use the following format:
```
[YYYY:MM:DD HH:MM:SS TZ] : : LEVEL : : APPNAME : : MESSAGE
```

#### Styles
To add some styles to the terminal. <br>
[Figlet](https://www.npmjs.com/package/figlet) to create some ascii and create a title when you run the server terminal.<br>
[Chalk](https://www.npmjs.com/package/chalk) to add colors

#### Documentation

[Swagger ui](https://www.npmjs.com/package/swagger-ui-express) <br>
[Swagger js doc](https://github.com/Surnet/swagger-jsdoc) <br>

Documentation. for Documentation API Use swagger to generate our documentation on code.

#### Storage
[Multer](https://www.npmjs.com/package/multer) <br>
[Formidable](https://www.npmjs.com/package/formidable) <br>

#### Security
[Jwt](https://www.npmjs.com/package/jsonwebtoken) for jwt based authentication. <br>

The sign in should be a different endpoint and once signed return a token. <br>
With this token we can gate any action the user tries on an auth middlewre that asks for the token. this token could go on the header. <br>
<br>
to save passwords we should encrypt them. some information about hash encryption <br>
There are other ways of handling security. like using a third party API like auth0. <br>
you can use something like Auth0, which is paid for more than 7k active users. Tutorial 1. Tutorial 2 <br>