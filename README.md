## HOW TO CREATE THE [Project Mgmt App](https://github.com/bradtraversy/project-mgmt-graphql) FROM SCRATCH

Author: IxxI5

##### YouTube: [GraphQL Crash Course With Full Stack MERN Project](https://www.youtube.com/watch?v=BcLNfwF04Kw)

##### Git (Code): [Project Mgmt App](https://github.com/bradtraversy/project-mgmt-graphql)

##### Gist (Queries): [GraphQL Queries & Mutations](https://gist.github.com/bradtraversy/fc527bc9a4659ab8de8e8066f3498723)

**Full Stack Web Application** _by Brad Traversy_

_It is a simple Project Management System, where we can add/manage clients, add projects, and connect projects to clients. The project page may show all the information including the client information as well. Furthermore, there is an area to update the details as well as a button to delete a project. In other words, the application provides full CRUD functionality as also it utilizes the Bootstrap library (modals) for the user to be able to add/edit new clients and projects._

**Backend:** GraphQL, ExpressJS with MongoDB database API.

**Frontend:** ReactJS, Apollo Client, Bootstrap.

---

**REST API vs GraphQL**

**REST API:** _Multiple endpoints running under an HTTP Server._

**GraphQL:** _It is a query language for APIs and a runtime for fulfilling those queries with existing (database) data. Access to the API is conducted through a single endpoint e.g. /graphql running under the graphqlHTTP Server (where there we send our queries)._

**Warning!** Steps content must be executed in the given order.

## **Step 1 - Creating an ATLAS MongoDB Database**

**Go to:** https://www.mongodb.com/cloud/atlas/register

- **Login or Create your ATLAS account**

- **Create a Project**

- **Deploy a Cloud Database: Free (Shared) | Create**

- **Cloud Provider & Region: aws**

- **Cluster Name: Cluster0 | Create Cluster**

**Create a database user using username and password | read/write privilege by default**

- **Create User**

- **Where would you like to connect from**

- **Add My Current IP Address | Finish and Close | Goto Database**

- **Browse Collections (on database creation completion)**

- **Add My Own Data**

- **Create Database | Database name: mgmt_db | Collection name: clients**

- **Cluster0 | Overview | Connect**

- **Connect using Mongo Compass | Copy the connection string e.g:**

  **mongodb+srv://username:<_password_>@cluster0.maxbpob.mongodb.net/mgmt_db**

- **Replace the username and password with those you created for MognoDB User**

- **Paste the above connection string to Mongo Compass**

- **Cluster0 | Overview | Connect**

- **Connect using your application | Copy the connection string e.g:**

  **mongodb+srv://username:<_password_>@cluster0.maxbpob.mongodb.net/mgmt_db?retryWrites=true&w=majority**

- **Replace the username and password with those you created for MognoDB User**

**Info:** Use Mongo Compass Desktop App to check your data: https://mongodb.com/products/compass

## **Step 2 - VS Code**

_Install Extensions_

- **VS Code ES7+ React/Redux/React-Native/JS snippets**

- **GitHub Copilot (non-mandatory)**

## **Step 3 - Server Side**

_Install Packages (npm ...) And Create Project's Structure (...)_

**VS Code Command Prompt Terminal**

```
mkdir project-mgmt-graphql

cd project-mgmt-graphql

npm init -y

npm i express express-graphql graphql mongoose cors colors

npm i -D nodemon dotenv

mkdir project-mgmt-app\server

cd server

mkdir config

mkdir models

mkdir schema

type nul > config/db.js

type nul > models/Client.js

type nul > models/Project.js

type nul > schema/schema.js

type nul > index.js

cd..

type nul > .env
```

## - Remarks (npm) | VS Code Terminal -

**npm init -y**

_creates the package.json (inside project-mgmt-graphql folder) file for the project's packages and dependencies._

**Note**: _As one may see in the Git Solution, project-mgmt-graphql/server there is also a copy of the package.json and package-lock.json, however with a slight difference (see below) in "scripts" key-values within the package.json file._

**npm i express express-graphql graphql mongoose cors colors**

**express**

_installs the express node.js package for server applications._

**express-graphql**

_installs the module to create an Express server that runs a GraphQL._

**graphql**

installs the query language for APIs placed on top of Mongoose.

**mongoose**

_installs the Object Data Modeling (ODM) library for MongoDB. Mongoose actually acts as the interface to connect/access MongoDB._

**cors**

installs the node.js package for providing a Connect/Express middleware that can be used to enable CORS with various options.

**colors**

installs text coloring capability for the console output.

**npm i -D nodemon dotenv**

**nodemon**

_installs the node.js tool as a dev-dependency that automatically restarts the node application when file changes have been detected._

**dotenv**

_installs the zero-dependency module that loads environment variables from a .env file into process.env._

## - Remarks (Folders and Files) | VS Code Explorer -

_Created Folders_

**project-mgmt-graphql**

**project-mgmt-app/server**

**project-mgmt-app/server/config**

**project-mgmt-app/server/models**

**project-mgmt-app/server/schema**

_Created Files_

```javascript
/* server/config/db.js: 
   Connects to ATLAS MongoDB through Mongoose. */
const mongoose = require("mongoose");
/*  ........
    ........
    ........
    ........
*/

module.exports = connectDB;
```

```javascript
/* server/models/Client.js:
   Defines the Client Model (similar to an SQL Table) in Mongoose. */
const mongoose = require('mongoose');

const ClientSchema = new mongoose.Schema({...})
/*  ........
    ........
    ........
    ........
*/
module.exports = mongoose.model('Client', ClientSchema);
```

```javascript
/* server/models/Project.js:
   Defines the Project Model (similar to an SQL Table) in Mongoose. */
const mongoose = require('mongoose');

const ProjectSchema = new mongoose.Schema({...})
/*  ........
    ........
   ........
    ........
*/
module.exports = mongoose.model('Project', ProjectSchema);
```

```javascript

/*  server/schema/schema.js
    (a) Creates the corrspendoing GraphQL ClientType and ProjectType based on
        Mongoose Project and Client models.
    (b) Configures GraphQL API in order to perform GET like actions (queries)
        for single or multiple ClientType and ProjectType requests.
    (c) Configures GraphQL API in order to perform POST, UPDATE and DELETE
        actions (mutations) for single or multiple ClientType and ProjectType
        requests. */
const Project = require('../models/Project');
const Client = require('../models/Client');

const {....} = require('graphql');

// Project Type
const ProjectType = new GraphQLObjectType({...})

// Client Type
const ClientType = new GraphQLObjectType({...})

// queries
const RootQuery = new GraphQLObjectType({...})

// Mutations
const mutation = new GraphQLObjectType({...})

module.exports = new GraphQLSchema({
  query: RootQuery,
  mutation,
});
```

```javascript
/* server/index.js:
   Server entry point. Server is running here. */
// dependencies
const express = require("express");
const colors = require("colors");
const cors = require("cors");

// utilize the .env file
require("dotenv").config();

// dependencies
const { graphqlHTTP } = require("express-graphql");
const schema = require("./schema/schema");
const connectDB = require("./config/db");
const port = process.env.PORT || 5000;

// initializing the express server and port number
const app = express();

// connect to MongoDB over Mongoose
connectDB();

// use cors middleware on Express Server
app.use(cors());

// use graphql middleware on Express Server
app.use(
  "/graphql",
  graphqlHTTP({
    schema,
    graphiql: process.env.NODE_ENV === "development",
  })
);

// listening (server is running) on process.env.PORT (PORT: 5000)
app.listen(port, console.log(`Server running on port ${port}`));
```

```javascript
/* project-mgmt-app/.env:
   File for the environment variables. */
NODE_ENV = "development";
PORT = 5000;
MONGO_URI =
  "mongodb+srv://username:<password>@cluster0.maxbpob.mongodb.net/mgmt_db?retryWrites=true&w=majority";

// Replace the username and password with those you created for MognoDB User
```

## **Step 4 - MongdoDB Atlas**

**Add My Own Data** | See example (JSON) below

```javascript
// Projects
{
    id: '1',
    clientId: '1',
    name: 'eCommerce Website',
    description:
      ' Aenean massa.  Donec quam felis, ultricies.',
    status: 'In Progress',
  },
  {
    id: '2',
    clientId: '2',
    name: 'Dating App',
    description:
      'Lorem ipsum dolor sit amet, consectetuer adipiscing elit.',
    status: 'In Progress',
  }

  // Clients
  {
    id: '1',
    name: 'Tony Stark',
    email: 'ironman@gmail.com',
    phone: '343-567-4333',
  },
  {
    id: '2',
    name: 'Natasha Romanova',
    email: 'blackwidow@gmail.com',
    phone: '223-567-3322',
  }
```

## **Step 5 - GraphiQL**

_GraphiQL is a graphical interactive in-browser GraphQL IDE_

**Chrome (or other Browser):** http://localhost:5000/graphql

**GraphQL Queries Test** | See below

```javascript
// Query (GET like request)
{
  client(id:"1") {
    name,
    email
  }
}

/* Server Response
{
  "data": {
  "client": {
  "name": "Tony Start",
  "email": "ironman@gmail.com"
    }
  }
}

GraphiQL: It returns just the data we want e.g. name and email
REST API: It returns everything e.g. id, name, email, phone
*/

// Mutation (POST like request)
mutation {
  addProject(name: "Test",
    description: "This is a description",
    status: progress,
    clientId:
      name,
      client {
        id
        name
  }
}

/* Server Response
{
  "data": {
  "addProject": {
  "name": "Test",
  "client": {
  "id": "6sdff512288ergeg252erg",
  "name": "Peter Parker"
      }
    }
  }
}

*/
```

**Info:** Let GraphQL Server running (PORT: 5000).

---

## **Step 6 - Client Side**

_Install Packages (npm ...) And Create Project's Structure (...)_

```
cd project-mgmt-app

** VS Code: Open a New (+) Terminal **

npx create-react-app client

cd client

npm i @apollo/client graphql react-router-dom react-icons

mkdir src\components

mkdir src\components\assets

mkdir src\mutations

mkdir src\pages

mkdir src\queries

type nul > index.css

del App.css

del logo.png

cd src/components/assets

** Add logo.png **

cd ..\..\..\

cd client

client/App.js ** Replace content with an empty JSX component | See below **

client/public/index.html: **AddCDN bootstrap links into index.html < head> | See below **

npm start
```

**Info:** Client (React) Server is now running but on PORT: 3000 (create/configure launch.json file for the browser accordingly).

**Fact:** We have two servers runnîng, one for the Backend (PORT: 5000) and another for the Frontend (PORT: 3000).

## - Remarks (Folders and Files) | VS Code Explorer -

**project-mgmt-app/client** Folder has been created automatically (npx ...).

_Created Folders_

**project-mgmt-app/client/src/components**

**project-mgmt-app/client/src/components/assets**

**project-mgmt-app/client/src/mutations**

**project-mgmt-app/client/src/pages**

**project-mgmt-app/client/src/queries**

_Modified Files_

```javascript
/* client/public/index.html:
   Add the CDN Bootstrap 5.3 links into <head> */
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>

/* NPM (alternatively as package)
React-Bootstrap

npm i react-bootstrap

https://github.com/react-bootstrap/code-sandbox-examples

Example:
*/

import { Jumbotron } from 'react-bootstrap';
/*.....*/
<Jumbotron>
  <div>Jumbtron Test</div>
</Jumbotron>
```

## **Step 7 - Client Side | Queries**

_Create GraphQL Queries | .js Files_

**project-mgmt-graphql/client/src/queries/clientQueries.js**

**project-mgmt-graphql/client/src/queries/projectQueries.js**

```javascript
/* src/queries/clientQueries.js:
   Provides the GET_CLIENTS gql`...` query (similar to a GET)
*/
import { gql } from "@apollo/client";

const GET_CLIENTS = gql`...`;

export { GET_CLIENTS };

// @apollo/client: It plays the role of the interface to the GraphQL Server, something similar of what Mongoose does for accessing the ATLAS MongoDB Database.
```

```javascript
/* src/queries/projectQueries.js:
   Provides the GET_PROJECT and GET_PROJECTS gql`...` queries (similar to a GET)
*/
import { gql } from "@apollo/client";

const GET_PROJECTS = gql`...`;

const GET_PROJECT = gql`...`;

export { GET_PROJECTS, GET_PROJECT };
```

## **Step 8 - Client Side | Mutations**

_Create GraphQL Mutations | .js Files_

**project-mgmt-graphql/client/src/mutations/clientMutations.js**

**project-mgmt-graphql/client/src/mutations/projectMutations.js**

```javascript
/* src/mutations/clientMutations.js:
   Provides the ADD_CLIENT and DELETE_CLIENT  gql`...` queries
   (similar to a POST)
*/
import { gql } from "@apollo/client";

const ADD_CLIENT = gql`...`;

const DELETE_CLIENT = gql`...`;

export { ADD_CLIENT, DELETE_CLIENT };
```

```javascript
/* src/mutations/projectMutations.js:
   Provides the ADD_PROJECT, DELETE_PROJECT and UPDATE_PROJECT 
   gql`...` queries (similar to POST and UPDATE)
*/
import { gql } from "@apollo/client";

const ADD_PROJECT = gql`...`;

const DELETE_PROJECT = gql`...`;

const UPDATE_PROJECT = gql`...`;

export { ADD_PROJECT, DELETE_PROJECT, UPDATE_PROJECT };
```

## **Step 9 - Client Side | React Components**

_Create React Components (.jsx files)_

**VS Code Command Prompt Terminal**

```
cd project-mgmt-app/client/src/components

type nul > AddClientModal.jsx

type nul > AddProjectModal.jsx

type nul > ClientInfo.jsx

type nul > ClientRow.jsx

type nul > Clients.jsx

type nul > DeleteProjectButton.jsx

type nul > EditProjectForm.jsx

type nul > Header.jsx

type nul > ProjectCard.jsx

type nul > Projects.jsx

type nul > Spinner.jsx
```

## - Remarks (.jsx files) | VS Code Explorer -

_Created Files (initially empty React Components)_

**AddClientModal.jsx**

**AddProjectModal.jsx**

**ClientInfo.jsx**

**ClientRow.jsx**

**Clients.jsx**

**DeleteProjectButton.jsx**

**EditProjectForm.jsx**

**Header.jsx**

**ProjectCard.jsx**

**Projects.jsx**

**Spinner.jsx**

```javascript
/* Use the rfce snippet to put some initial content to React Components e.g. */

// rfce
function ComponentName() {
  return <div>ComponentName</div>;
}

export default ComponentName;
```

```javascript
/* Useful React Snippets */

import React from "react";                          // imr
import React, { useState, useEffect } from "react"; // imrse
import moduleName from "module";                    // imp
import { } from "module";                           // imd

console.log(object);                                // clg
const first = (second) => { third }                 // nfn

// rfc
export default function name() {
  return (
    <div>name</div>
  )
}

// rfce
function name() {
  return (
    <div>name</div>
  )
}

export default name

// rafce
const name = () => {
  return (
    <div>name</div>
  )
}

export default name
```

```javascript
/* client/App.js: initially empty JSX component
   Snippet: rfce (or rfc) -> creates a React Component as follow
*/
import React from "react"; // not required anymore -> remove it

function App() {
  /*....your code or add the below for start.... */
  return <div>Hello</div>;
}

export default App;
```

```javascript
/*  components/AddClientModal.jsx:
    Bootstrap modal dialog for adding a Client
*/
import { useState } from "react";
import { FaUser } from "react-icons/fa";
import { useMutation } from "@apollo/client";
import { ADD_CLIENT } from "../mutations/clientMutations";
import { GET_CLIENTS } from "../queries/clientQueries";

export default function AddClientModal() {
  /*
    useState for the inputs
    useMutation destructuring for executing the ADD_CLIENT
    onSubmit event Handler
  */

  return <>{/* Bootstrap modal html */}</>;
}

/*
https://getbootstrap.com/docs/5.3/components/modal/

-Copy the html into AddClientModal return
-Don't forget to replace class with className
*/
```

```javascript
/*  components/AddProjectModal.jsx:
    Bootstrap modal dialog for adding a Project
*/
import { useState } from "react";
import { FaList } from "react-icons/fa";
import { useMutation, useQuery } from "@apollo/client";
import { ADD_PROJECT } from "../mutations/projectMutations";
import { GET_PROJECTS } from "../queries/projectQueries";
import { GET_CLIENTS } from "../queries/clientQueries";

export default function AddProjectModal() {
  /*
    useState for the inputs
    useMutation destructuring for executing the ADD_PROJECT
    onSubmit event Handler
  */

  return <>{/* Bootstrap modal html */}</>;
}

/*
https://getbootstrap.com/docs/5.3/components/modal/

-Copy the html into AddClientModal return
-Don't forget to replace class with className
*/
```

```javascript
/*  components/ClientInfo.jsx:
    Client Information styling
*/
import { FaEnvelope, FaPhone, FaIdBadge } from "react-icons/fa";

export default function ClientInfo({ client }) {
  return <>{/* hmtl and react components */}</>;
}
```

```javascript
/*  components/ClientRow.jsx:
    Client Row styling and delete Client functionality
*/
export default function ClientRow({ client }) {
  /*
    useMutation destructuring for executing the DELETE_CLIENT
  */

  return <>{/* <tr> ....</tr> */}</>;
}
```

```javascript
/*  components/Clients.jsx:
    Executes the GET_CLIENTS Query and displays the results 
    in a table form
*/
import { useQuery } from "@apollo/client";
import ClientRow from "./ClientRow";
import Spinner from "./Spinner";
import { GET_CLIENTS } from "../queries/clientQueries";

export default function Clients() {
  /*
    useQuery to retrieve data over GraphQL queries
    display a Spinner (component) until data have been
    fetched
  */

  return <>{/* html plus ClientRow component */}</>;
}
```

```javascript
/*  components/DeleteProjectButton.jsx:
    Deletes a Project through a button
*/
import { useNavigate } from "react-router-dom";
import { FaTrash } from "react-icons/fa";
import { DELETE_PROJECT } from "../mutations/projectMutations";
import { GET_PROJECTS } from "../queries/projectQueries";
import { useMutation } from "@apollo/client";

export default function DeleteProjectButton({ projectId }) {
  /*
    useMutation to execute the DELETE_PROJECT and refetch
    useNavigate, on delete completion go back to Home page 
  */

  return <>{/* html */}</>;
}
```

```javascript
/*  components/EditProjectForm.jsx:
    Edits a Project using a Bootstrap Form styling
*/
import { useState } from "react";
import { useMutation } from "@apollo/client";
import { GET_PROJECT } from "../queries/projectQueries";
import { UPDATE_PROJECT } from "../mutations/projectMutations";

export default function EditProjectForm({ project }) {
  /*
  useState for the form input fields
  useMutation for the execution of UPDATE_PROJECT and refetch
  onSubmit event Handler
*/

  return <>{/* html */}</>;
}
/*
https://getbootstrap.com/docs/5.3/forms/form-control/

-Copy the html into EditProjectForm return
-Don't forget to replace class with className
*/
```

```javascript
/*  components/Header.jsx:
    Header styling (App's name and logo)
*/
import logo from "./assets/logo.png";

export default function Header() {
  return <>{/* html */}</>;
}
```

```javascript
/*  components/ProjectCard.jsx:
    Project Card styling
*/
export default function ProjectCard({ project }) {
  return <>{/* html */}</>;
}
```

```javascript
/*  components/Projects.jsx:
    Queries the Projects and displays them as Cards
*/
import Spinner from "./Spinner";
import { useQuery } from "@apollo/client";
import ProjectCard from "./ProjectCard";
import { GET_PROJECTS } from "../queries/projectQueries";

export default function Projects() {
  /*
    useQuery to retrieve the data (Projects)
    display a Spinner (component) until data have been
    fetched
  */

  return <>{/* html plus ProjectCard component */}</>;
}
```

```javascript
/*  components/Spinner.jsx:
    Displays a Spinner until data have been fetched
*/
export default function Spinner() {
  return <>{/* html */}</>;
}
```

## **Step 10 - Client Side | Pages**

_Created Files (initially empty React Components)_

**client/src/pages/Home.jsx**

**client/src/pages/NotFound.jsx**

**client/src/pages/Project.jsx**

## - Remarks (.jsx files) | VS Code Explorer -

_Created Files (initially empty React Components)_

**Home.jsx**

**NotFound.jsx**

**Project.jsx**

```javascript
/* Use the rfce snippet to put some initial content to React Components e.g. */

// rfce
function ComponentName() {
  return <div>ComponentName</div>;
}

export default ComponentName;
```

```javascript
/* src/pages/Home.jsx:
   Creates the Home page that consists of React Components as the
   AddClientModal, AddProjectModal, Projects and Clients
*/
import Clients from "../components/Clients";
import Projects from "../components/Projects";
import AddClientModal from "../components/AddClientModal";
import AddProjectModal from "../components/AddProjectModal";

export default function Home() {
  return <>{/* React Components */};</>;
}
```

```javascript
/* src/pages/NotFound.jsx:
   On not found URL, it goes to the NotFound page. Moreover, a button
   to go back to Home page '/' is provided
*/
import { FaExclamationTriangle } from "react-icons/fa";
import { Link } from "react-router-dom";

export default function NotFound() {
  return <>{/* React Components */};</>;
}
```

```javascript
/* src/pages/Project.jsx:
   Creates the Project Page for editing a Project 
*/
import { Link, useParams } from "react-router-dom";
import Spinner from "../components/Spinner";
import ClientInfo from "../components/ClientInfo";
import DeleteProjectButton from "../components/DeleteProjectButton";
import EditProjectForm from "../components/EditProjectForm";
import { useQuery } from "@apollo/client";
import { GET_PROJECT } from "../queries/projectQueries";

export default function Project() {
  return <>{/* html plus React Components */}</>;
}
```

## - Remarks (.js files) | VS Code Explorer -

_Modified Files_

**client/src/index.js**

**client/src/App.js**

```javascript
/* client/src/index.js:
   This is the entry point for the Frontend (client)
*/

import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

```javascript
/* client/src/App.js:
   The entire App is launced from here
*/
import { BrowserRouter as Router, Route, Routes } from "react-router-dom";
import Header from "./components/Header";
import { ApolloProvider, ApolloClient, InMemoryCache } from "@apollo/client";
import Home from "./pages/Home";
import Project from "./pages/Project";
import NotFound from "./pages/NotFound";

const cache = new InMemoryCache({
  /*...*/
});

const client = new ApolloClient({
  uri: "http://localhost:5000/graphql",
  cache,
});

function App() {
  return (
    <>
      <ApolloProvider client={client}>
        <Router>
          <Header />
          <div className="container">
            <Routes>
              <Route path="/" element={<Home />} />
              <Route path="/projects/:id" element={<Project />} />
              <Route path="*" element={<NotFound />} />
            </Routes>
          </div>
        </Router>
      </ApolloProvider>
    </>
  );
}

export default App;

/* ApolloProvider: It is wrapper around the Router and all other React 
   Components, in order to provide the GraphQL functionality globally.
   InMemoryCache: It provides data caching/updating in memory without the
   need of refetching the data in order to synchronize the UI with the MongoDB.
   Routes: It provides the distinguishable Route/s to navigate across the App.
*/
```

## **Step 11 - Client Side | Bootstrap**

_CSS Bootstrap Classes Equivalents_

https://www.w3schools.com/bootstrap/bootstrap_ref_all_classes.asp

_Bootstrap 5.3_

https://getbootstrap.com/docs/5.3/getting-started/introduction/
