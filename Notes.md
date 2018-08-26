# MERN Stack Introduction

## Setup Backend (Mongo, Express, NodeJS)

[Full Project](https://github.com/bradtraversy/devconnector) : from Udemy

### Install the following Plugins for VSCode

1.  Prettier
2.  Live Server
3.  Bracket Pair Colorizer
4.  ES7 React Redux GraphQL

### Setup Directory (devconnector)

1.  install npm package.json file
2.  express
3.  mongoose
4.  passport
5.  passport-jwt
6.  jsonwebtoken
7.  body-parser
8.  bcryptjs
9.  validator

run `npm install --save`

### Install Dev Dependencies

1.  nodemon

### Create the server entry point

1.  create a `server.js` file

```javascript
const express = require("express");
const app = express();

app.get("/", () => console.log("index"));
```

### Create and connect to mongo

Creating an exports module for dynamic configuration

```javascript
module.exports = {
  mongoDbURI: process.env.mongoDBURI || ""
};
```

server.js

```javascript
const port = process.env.PORT || 5000;
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

### Continuation: Changing to Modularized Code

Now that we have a basic skeleton that boots up, we can split out the different routes into their own files.

server.js

```javascript
const express = require("express");
const mongoose = require("mongoose");

const users = require("./routes/api/users");
const profile = require("./routes/api/profile");
const posts = require("./routes/api/posts");

const app = express();

// DB Config
const db = require("./config/keys").mongoURI;
mongoose
  .connect(db)
  .then(() => console.log("MongoDB Connected"))
  .catch(err => console.log(err));

app.get("/", (req, res) => res.send("hello"));

// Use Routes
app.use("/api/user", users);
app.use("/api/profile", profile);
app.use("/api/posts", posts);

const port = require("./config/keys").port;

app.listen(port, () => console.log(`Running on port: ${port}`));
```

Note the `app.use` portion.

```javascript
// Use Routes
app.use("/api/user", users);
app.use("/api/profile", profile);
app.use("/api/posts", posts);
```

This allows us to assign these objects as the designated handlers for these routes.

users.js

```javascript
const express = require("express");
const router = express.Router();

router.get("/test", (req, res) =>
  res.json({
    msg: "user"
  })
);

module.exports = router;
```

Grab the express router and define a `get` request on the `/test` router which calls a callback function responsding with the json data.

Note how we export the router since we modified the `router` object to include additional routes.

### Creating User Model and JWT Protected Routes

#### Convention for MongoDB

1.  Uppercased file name `User.js` as an example
