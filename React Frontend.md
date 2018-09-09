# Setting up React Application

## Initial Development Setup

Create React App and Concurrently (to run both dev web site and dev server together)

- concurrently allows the execution of more than one terminal command

### Setup

```
npm i concurrently
```

inside `package.json` in the `server` folder, we create a couple of scripts

```json
  "scripts": {
    "client-install": "npm install --prefix client",
    "start": "node server.js",
    "server": "nodemon server.js",
    "client": "npm start --prefix client",
    "dev": "concurrently \"npm run server\" \"npm run client \"",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```

Create a `client-install` script that will go into the client folder and 'install' all the node modules required to run a development react server.

We then create an optional `client` start command which will only run the web server not the backend

We finally create a `dev` command which will use concurrently to execute and run both the web and backend servers.

Then to start the server and web simply run the command...

```
npm run dev
```

---

## Setup Bootstrap and Assets

Go to [Get Boostrap](https://getbootstrap.com)

copy the following and install in `index.html`

```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">

<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>
```

In `src` folder, create another folder called `components`

### Setup Router

Install the React Router v 4

```
npm i react-router-dom
```

Setup Router inside `app.js`

```javascript
import React, { Component } from "react";
import { BrowserRouter as Router }
import "./App.css";
import Navbar from "./components/layout/Navbar";
import Footer from "./components/layout/Footer";
import Landing from "./components/layout/Landing";
class App extends Component {
  render() {
    return (
      <div className="App">
        <Navbar />
        <Landing />
        <Footer />
      </div>
    );
  }
}

export default App;
```

Currently whenever the app opens we will always open landing, however we want to create a route for this

```javascript
class App extends Component {
  render() {
    return (
      <Router>
        <div className="App">
          <Navbar />
          <Route path="/" component={Landing} />
          <Footer />
        </div>
      </Router>
    );
  }
}
```

## Special Notes: Binding of `this`

There are several ways to get `this` to come with a function. One of the ways used in the course is inside the class constructor.

```javascript
class Example {
  constructor() {
    super();
    // we can bind to this here
    this.myFunction = this.myFunction.bind(this);
    this.someOtherFunction = this.someOtherFunction.bind(this);
  }

  // this is not exclusively inserted into this function scope
  myFunction(e) {
    this.someOtherFunction(e);
  }

  someOtherFunction(e);
}
```

The alternative method to this is to use a fat arrow function

```javascript
this.onChange = this.onChange.bind(this);

const onChangeNoBind = e => {
  // this is now in scope for this function
};

onChange(e) {
  // this is in scope because we bound this to the function
}
```

---

# Setting up Validation

In order to make / apply multiple classes, we need a package called `classNames` since React does not by default support on the fly className configurations

```
npm i classnames
```

This will allow us to apply multiple classnames onto objects such as below

```javascript
import React, { Component } from "react";
import classnames from "classnames";
class Register extends Component {
  constructor() {
    super();
    this.state = {
      name: "",
      email: "",
      password: "",
      confirmPassword: "",
      errors: {}
    };

    // Bind 'this' to the onchange function
    this.onChange = this.onChange.bind(this);
    this.onSubmit = this.onSubmit.bind(this);
  }
  /**
   *
   * @param {object} e event of the text field input
   */
  onChange(e) {
    this.setState({ [e.target.name]: e.target.value });
  }

  onSubmit(e) {
    e.preventDefault();
    const newUser = {
      name: this.state.name,
      email: this.state.email,
      password: this.state.password,
      confirmPassword: this.state.confirmPassword
    };
    console.log(newUser);
  }

  render() {
    const { errors } = this.state;
    return (
      <div className="register">
        <div className="container">
          <div className="row">
            <div className="col-md-8 m-auto">
              <h1 className="display-4 text-center">Sign Up</h1>
              <p className="lead text-center">Create your DevConnect Account</p>
              <form noValidate onSubmit={this.onSubmit}>
                <div className="form-group">
                  <input
                    type="text"
                    className={classnames("form-control form-control-lg", {
                      "is-invalid": errors.name
                    })}
                    placeholder="First and Last Name"
                    name="name"
                    value={this.state.name}
                    onChange={this.onChange}
                  />
                  {errors.name && (
                    <div className="invalid-feedback">{errors.name}</div>
                  )}
                </div>
                <div className="form-group">
                  <input
                    type="email"
                    className={classnames("form-control form-control-lg", {
                      "is-invalid": errors.email
                    })}
                    placeholder="Email Address"
                    name="email"
                    value={this.state.email}
                    onChange={this.onChange}
                  />
                  {errors.email && (
                    <div className="invalid-feedback">{errors.email}</div>
                  )}
                </div>
                <div className="form-group">
                  <input
                    type="password"
                    className={classnames("form-control form-control-lg", {
                      "is-invalid": errors.password
                    })}
                    placeholder="Password"
                    name="password"
                    value={this.state.password}
                    onChange={this.onChange}
                  />
                  {errors.password && (
                    <div className="invalid-feedback">{errors.password}</div>
                  )}
                </div>
                <div className="form-group">
                  <input
                    type="password"
                    className={classnames("form-control form-control-lg", {
                      "is-invalid": errors.confirmPassword
                    })}
                    placeholder="Password"
                    name="confirmPassword"
                    value={this.state.confirmPassword}
                    onChange={this.onChange}
                  />
                  {errors.confirmPassword && (
                    <div className="invalid-feedback">
                      {errors.confirmPassword}
                    </div>
                  )}
                </div>
                <input type="submit" className="btn btn-info btn-block mt-4" />
              </form>
            </div>
          </div>
        </div>
      </div>
    );
  }
}
export default Register;
```
