# Dev - Ops Guides

This repository contains personal guides for Backend Web Application development.

## Backend Frameworks Guides
- [Nodejs]()
- [SpringBoot]()
- [FastAPI]()

## General Backend Application Design Steps

### I. DESIGN

The first thing to have in mind it's to know what you want to builf, functionalities, options, operations etc, at least the core of the application because based on this application sketch we have to design our database.

### II. DB & STRUCTURE

The first thing you should have, its the DB structure. How Data is going to be managed for the system and craft our relational model. its better to have a plan before going to develop and then avoid problems to change the structure later. <br>
<br>
Its recomended that the functionalities of the frontend have been planned already, that we already know how the site its going to work and communicate. that way we can then easily plan the DB and then plan how to develop an API, and at that stage the Frontend devs can work with it to start and backend can continue with the rest of the backend, be it, more complex parts than a restAPI, like processing etc. <br>
<br>
With the DB planned, lets start with the server itself. The backend project we should separate some basic conecpts to keep everything in order. the following structure its a good way to keep everything orderly. <br>
<br>
Folder Structure:
```
    .env            Set up String constants and credentials
    configurations/ Global app configurations and constants
    controllers/    Endpoints and functions controllers
    services/       Functions implementations (Controller implements)
    database/       SQl Definition & ORM config
    lib/            Personal Libs and helper functions
    models/         Data Models
    public/         Public Resources and frontend goes here
    resources/      Backend Resources and info that doesnt go on the DB
    routes/         Api routes definitions (They call the controllers)
    main.lan        Main App config
```
<br>
Some of this folders may not be necessary depending on your project. for example a configurations folder  may be redundant because you have all constants as strings read from .env. but other times you may need singleton constants and therefore use extra configurations across the whole app. <br>
<br>
An .env file is essential. here you set up the credential for your DB, IPs for APIs, keys, etc. (use the .ENV library). However, this file keep it on .gitignore to not leak your sensible information, instead add a .env.BASE file with template information and explain to use it as template to create the .env file. <br>

### III. VERSION CONTROL

Before we start developing it's essential to have a version control strategy in mind. even if it's a simple single branch. at least initialize the git repository and set up gitignore. <br>
<br>
Branch strategyA simple branch startegy could be simply working on a single branch. however another strategy may involve multiple branches as following.Main branch. this branch should only be touched to merge changes from another branch. this should be the stable branch and only updated when something was tested.Development. Main development branch. unstable and used to  work on new functionalities or developments.Personal Branches. personal development branches used when multiple people are working on the same project.Fix temporal branches. Temporal branches to experiment fixes or new things that could break the stable.stale branches. branches with stable old versions of the code. <br>
<br>
Project
```
    Main___Development____PersonalBranches___FixTemporal
             \         \___PersonalBranches___FixTemporal
              \         \__PersonalBranches___FixTemporal
               \_____StaleBranchV1
                               \
                                \_____StaleBranchV2
```

### IV. DOCUMENTATION

Another thing to have it's how to document your code and project  to make it easier for other people to work with it or to look at it time later and get how it works.
- Well detailed Instructions for System installing, configuration, and deployment.
- Simple diagrams of the system structure. for the general design of the application and if posible, for more specific modules, and for process flows (that change over time). this could be very useful to understand the system more than explaining it. 
- Also description of the components and how do they work.
- components and functions inventory.
- Error documentation.

### V. COMMENTING

Keep the code well explained. when a function does something difficult try to explain briefly the input parameters, output result, how does it do it. but dont overdo it. simple functions dont need to be well explained. just overall structural points and description of complex functions and points
- Use a structure for function definitions. and use extensions like better comments to add
- For documenting and testing endpoints on a server, use swagger-doc
- Extensions for commenting.Docstring (Python)Add jsdoc comments (JavaScript)Better comments

### VI. DEVELOPMENT

Now it's time to create the server itself. <br>
Create the project. Use your preferred framework to create the basic structure, be it nextjs with create-next-app, django cli app to create the project, spring initializr, or node init and creating yourself the basic structure like with express. <br>
<br>
Re-arrange, or create the folders based on the structure defined before and start the git repository to start working. (Don't forget the .env)<br>
Create a gitignore file to ignore heavy files on the project. modules from node are also important to ignore and configuration files with sensitive data. example:
```sh
node_modules/*
.log
mydatabase_backup.sql
nohup.out
.env
```
Install the basic dependencies. (You may still add them later if you need) like for reading .env files, handle requests, loggers, security, etc, but more importantly, to handle your DB.<br>
Prepare the DB system. To keep state we need a database system. the are many Database systems that we can choose and some of them use very different approaches and technologies. most common technologies are SQL, Document Based, Key-value DBs, etc. and some of the most common Database systems are:
    SQL:
        - MySQL
        - MariaDB 
        - PostgreSQL
        - Oracle
        - SQLServer
    Document:
        - mongodb
        - firebase
        - supabase
There are many other database paradigms, like document for search like elastic search, key-value for cache like Redis and many, many more. the world of DB systems its a vast one with some systems design to run both on bare metal and cloud (Like most SQL systems) and some are cloud services (Like firebase, supabase, etc). [more info](https://www.youtube.com/watch?v=W2Z7fbCLSTw). <br>
<br>
We probably already chose a DB system because to be at this point we should had already design our DB and designing a document based DB its very different from a relational or a graph DB.
However, we need our code to comunicate to our DB system and for that we should use a bridge, an ORM for relational DBs or a ODM for document based DBs. and with it create the entity models and relations of our DB. <br>
<br>
Set up Logging. Use a well established logging library. and use correct levels for logging info. Use Debug to indicate points in that give information about errors. but use other levels for inform the developer about the components running, so it can be easy to turn on or off this information and don't saturate the console.A good format for logging info. with date and time and component that output the log. Format:[YYYY:MM:DD HH:MM:SS TZ] : : LEVEL : : APPNAME : : MESSAGE. <br>
<br>
Add some style to the terminal. Use figlet to create some ascii and create a title when you run the server terminal. Forms and color with figlet and terminal color libraries. <br>
However, Styles are cool but should be used only in development. for production the logs should be json format and shouldnt have styles. <br>
<br>
Documentation for our API. Use swagger to generate our documentation on code to keep everything organized and nicely commented so it will be easier for us and other people to keep track of our code adn to generate a ui where to test our API easily. [Swagger](https://www.youtube.com/watch?v=S8kmHtQeflohttps://www.youtube.com/watch?v=apouPYPh_as).
<br>
Security. using token authentication mecanism with jwt.for our login we need to signup first. <br>
The sign in should be a different endpoint and once signed return a token.<br>
With this token we can gate any action the user tries on an auth middlewre that asks for the token. this token could go on the header.<br>
to save passwords we should encrypt them. some information about hash encryption<br>
https://sectigostore.com/blog/hashing-vs-encryption-the-big-players-of-the-cyber-security-world/
There are other ways of handling security. like using a third party API like auth0.<br>
<br>
Beyond CRUD functionalities.
Users levels.
Users activity registry