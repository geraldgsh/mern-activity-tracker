This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `yarn eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `yarn build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify


backend$ npm install express cors mongoose dotenv
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN backend@1.0.0 No description
npm WARN backend@1.0.0 No repository field.

+ dotenv@8.2.0
+ express@4.17.1
+ cors@2.8.5
+ mongoose@5.9.28
added 81 packages from 50 contributors and audited 81 packages in 10.247s

2 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


$ npm install -g nodemon

> nodemon@2.0.4 postinstall /home/ggoh/.nvm/versions/node/v14.5.0/lib/node_modules/nodemon
> node bin/postinstall || exit 0

+ nodemon@2.0.4


$ sudo npm install -g nodemon
[sudo] password for ggoh:
/usr/bin/nodemon -> /usr/lib/node_modules/nodemon/bin/nodemon.js

> nodemon@2.0.4 postinstall /usr/lib/node_modules/nodemon
> node bin/postinstall || exit 0

Love nodemon? You can now support the project via the open collective:
 > https://opencollective.com/nodemon/donate

{"os":"linux","arch":"x64"})

+ nodemon@2.0.4
added 120 packages from 54 contributors in 11.337s


```js
.\backend\server.js

const express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');

require('dotenv').config();

const app = express();
const port = process.env.PORT || 5500;

app.use(cors());
app.use(express.json());

app.listen(port, () => {
  console.log(`Server is running on port:${port}`);
});
```


backend$ nodemon server
[nodemon] 2.0.4
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node server.js`
Server is running on port:5500


```js
.\backend\server.js

  
const express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');

require('dotenv').config();

const app = express();
const port = process.env.PORT || 5000;

app.use(cors());
app.use(express.json());

const uri = process.env.ATLAS_URI;
mongoose.connect(uri, { useNewUrlParser: true, useCreateIndex: true }
);
const connection = mongoose.connection;
connection.once('open', () => {
  console.log("MongoDB database connection established successfully");
})

app.listen(port, () => {
  console.log(`Server is running on port: ${port}`);
});
```

atlas > connect

Complete the setup by;
1. Whitelisting IP address
2. Setup username and password
3. Next

Add your connection string into your application code > copy link

Create DB Schema

```js
\backend\models\user.model.js

const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const userSchema = new Schema({
  username: {
    type: String,
    required: true,
    unique: true,
    trim: true,
    minlength: 3
  },
}, {
  timestamps: true,
});

const User = mongoose.model('User', userSchema);

module.exports = User;
```


```js
.\backend\models\user.exercise.js

const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const exerciseSchema = new Schema({
  username: { type: String, required: true },
  description: { type: String, required: true },
  duration: { type: Number, required: true },
  date: { type: Date, required: true },
}, {
  timestamps: true,
});

const Exercise = mongoose.model('Exercise', exerciseSchema);

module.exports = Exercise;
```


Create routes;

```js
\backend\server.js

.
.
const exercisesRouter = require('./routes/exercises');
const usersRouter = require('./routes/users');

app.use('/exercises', exercisesRouter);
app.use('/users', usersRouter);
.
.
```


```js
\backend\routes\users.js

  
const router = require('express').Router();
let User = require('../models/user.model');

router.route('/').get((req, res) => {
  User.find()
    .then(users => res.json(users))
    .catch(err => res.status(400).json('Error: ' + err));
});

router.route('/add').post((req, res) => {
  const username = req.body.username;

  const newUser = new User({username});

  newUser.save()
    .then(() => res.json('User added!'))
    .catch(err => res.status(400).json('Error: ' + err));
});

module.exports = router;
```


```js
\backend\routes\exercises.js

const router = require('express').Router();
let Exercise = require('../models/exercise.model');

router.route('/').get((req, res) => {
  Exercise.find()
    .then(exercises => res.json(exercises))
    .catch(err => res.status(400).json('Error: ' + err));
});

router.route('/add').post((req, res) => {
  const username = req.body.username;
  const description = req.body.description;
  const duration = Number(req.body.duration);
  const date = Date.parse(req.body.date);

  const newExercise = new Exercise({
    username,
    description,
    duration,
    date,
  });

  newExercise.save()
  .then(() => res.json('Exercise added!'))
  .catch(err => res.status(400).json('Error: ' + err));
});

router.route('/:id').get((req, res) => {
  Exercise.findById(req.params.id)
    .then(exercise => res.json(exercise))
    .catch(err => res.status(400).json('Error: ' + err));
});

router.route('/:id').delete((req, res) => {
  Exercise.findByIdAndDelete(req.params.id)
    .then(() => res.json('Exercise deleted.'))
    .catch(err => res.status(400).json('Error: ' + err));
});

router.route('/update/:id').post((req, res) => {
  Exercise.findById(req.params.id)
    .then(exercise => {
      exercise.username = req.body.username;
      exercise.description = req.body.description;
      exercise.duration = Number(req.body.duration);
      exercise.date = Date.parse(req.body.date);

      exercise.save()
        .then(() => res.json('Exercise updated!'))
        .catch(err => res.status(400).json('Error: ' + err));
    })
    .catch(err => res.status(400).json('Error: ' + err));
});

module.exports = router;
```


Use Postman to test API POST and GET

```sh

{
  "username": "bold"
}

```

POST http://localhost:5000/users

```sh

User added!

```

GET http://localhost:5000/users

```sh
[
    {
        "_id": "5f2f961adcc4a909c9ca29d7",
        "username": "bold",
        "createdAt": "2020-08-09T06:22:18.163Z",
        "updatedAt": "2020-08-09T06:22:18.163Z",
        "__v": 0
    },
    {
        "_id": "5f2f9634dcc4a909c9ca29d8",
        "username": "bolder",
        "createdAt": "2020-08-09T06:22:44.078Z",
        "updatedAt": "2020-08-09T06:22:44.078Z",
        "__v": 0
    }
]
```


POST http://localhost:5000/exercises/add

```sh
{
	"username": "bold",
	"description": "run",
	"duration": 45,
	"date": "2020-08-09T06:22:18.163Z"
}
```

```sh
"Exercise added!"
```


GET http://localhost:5000/exercises/

```sh
[
    {
        "_id": "5f2f9cd3dcc4a909c9ca29db",
        "username": "bold",
        "description": "run",
        "duration": 45,
        "date": "2020-08-09T06:22:18.163Z",
        "createdAt": "2020-08-09T06:50:59.821Z",
        "updatedAt": "2020-08-09T06:50:59.821Z",
        "__v": 0
    },
    {
        "_id": "5f2f9d2bdcc4a909c9ca29dc",
        "username": "bold",
        "description": "swim",
        "duration": 40,
        "date": "2020-08-08T06:22:18.163Z",
        "createdAt": "2020-08-09T06:52:27.007Z",
        "updatedAt": "2020-08-09T06:52:27.007Z",
        "__v": 0
    }
]

```


GET http://localhost:5000/exercises/5f2f9cd3dcc4a909c9ca29db/id

```sh
{
	"username": "bold",
	"description": "swim",
	"duration": 40,
	"date": "2020-08-08T06:22:18.163Z"
}
```

POST http://localhost:5000/exercises/update/5f2f9cd3dcc4a909c9ca29db/id

```sh
{
    "username": "bold",
    "description": "walk",
    "duration": 93,
    "date": "2020-08-08T06:22:18.163Z"
}
```

```sh
"Exercise updated!"
```


GET http://localhost:5000/exercises/5f2f9cd3dcc4a909c9ca29db/id

```sh
{
	"username": "bold",
	"description": "swim",
	"duration": 93,
	"date": "2020-08-08T06:22:18.163Z"
}
```

DELETE http://localhost:5000/exercises/5f2f9cd3dcc4a909c9ca29db/id

```sh
"Exercise deleted!"
```


Front end

Install bootstrap

```sh
mern-activity-tracker$ npm install popper.js
npm WARN deprecated popper.js@1.16.1: You can find the new Popper v2 at @popperjs/core, this package is dedicated to the legacy v1
npm notice created a lockfile as package-lock.json. You should commit this file.
+ popper.js@1.16.1
added 1 package from 2 contributors and audited 1 package in 1.291s

1 package is looking for funding
  run `npm fund` for details

found 0 vulnerabilities


mern-activity-tracker$ npm install jquery
+ jquery@3.5.1
added 1 package from 1 contributor and audited 2 packages in 1.107s

1 package is looking for funding
  run `npm fund` for details

found 0 vulnerabilities


mern-activity-tracker$ npm install bootstrap
+ bootstrap@4.5.2
added 1 package from 2 contributors and audited 3 packages in 3.038s

2 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```


Import bootstrap

```js
/src/App.js

.
.
import "bootstrap/dist/css/bootstrap.min.css";
.
.
```


Install router react dom

```sh

$ npm install react-router-dom
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN react-router-dom@5.2.0 requires a peer of react@>=15 but none is installed. You must install peer dependencies yourself.
npm WARN react-router@5.2.0 requires a peer of react@>=15 but none is installed. You must install peer dependencies yourself.
npm WARN mini-create-react-context@0.4.0 requires a peer of react@^0.14.0 || ^15.0.0 || ^16.0.0 but none is installed. You must install peer dependencies yourself.

+ react-router-dom@5.2.0
added 18 packages from 11 contributors and audited 18 packages in 7.884s
found 0 vulnerabilities

```

Add routes in App.js

```js
/src/App.js

.
.
function App() {
  return (
    <Router>
      <div className="container">
      <Navbar />
      <br/>
      <Route path="/" exact component={ExercisesList} />
      <Route path="/edit/:id" component={EditExercise} />
      <Route path="/create" component={CreateExercise} />
      <Route path="/user" component={CreateUser} />
      </div>
    </Router>
  );
}
.
.
```

Import cpmpnents in App.js

```js
/src/App.js

.
.
import Navbar from "./components/navbar.component"
import ExercisesList from "./components/exercises-list.component";
import EditExercise from "./components/edit-exercise.component";
import CreateExercise from "./components/create-exercise.component";
import CreateUser from "./components/create-user.component";
.
.
```

Create component folders and files

```
mern-activity-tracker
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    ├── serviceWork.js
    ├── setupTests.js
    └── components
        ├── create-exercise.component.js
        ├── create-user.component.js
        ├── edit-exercise.component.js
        ├── exercises-list.component.js
        └── navbar.component.js
```


Setup navbar component

```js
/src/components/navbar.component.js 

import React, { Component } from 'react';
import { Link } from 'react-router-dom';

export default class Navbar extends Component {

  render() {
    return (
      <nav className="navbar navbar-dark bg-dark navbar-expand-lg">
        <Link to="/" className="navbar-brand">ExcerTracker</Link>
        <div className="collpase navbar-collapse">
        <ul className="navbar-nav mr-auto">
          <li className="navbar-item">
          <Link to="/" className="nav-link">Exercises</Link>
          </li>
          <li className="navbar-item">
          <Link to="/create" className="nav-link">Create Exercise Log</Link>
          </li>
          <li className="navbar-item">
          <Link to="/user" className="nav-link">Create User</Link>
          </li>
        </ul>
        </div>
      </nav>
    );
  }
}
```


Setup exercise list component

```js
/src/components/exercises-list.component.js

export default class ExercisesList extends Component {
    render() {
    return (
      <div>
        <p>You are on the Exercises List component!</p>
      </div>
    )
  }
}
```

Setup edit exercise list component

```js
/src/components/edit-exercise.component.js

export default class EditExercises extends Component {
    render() {
    return (
      <div>
        <p>You are on the Edit Exercises component!</p>
      </div>
    )
  }
}
```

Setup create exercise list component

```js
/src/components/edit-exercise.component.js

export default class CreateExercises extends Component {
    render() {
    return (
      <div>
        <p>You are on the Create Exercises component!</p>
      </div>
    )
  }
}
```

Setup create user component

```js
/src/components/edit-exercise.component.js

export default class CreateUsers extends Component {
    render() {
    return (
      <div>
        <p>You are on the Create Users component!</p>
      </div>
    )
  }
}
```


Setup create exercise list constructor

```js
/src/components/create-exercise.component.js

export default class EditExercise extends Component {
  constructor(props) {
    super(props)
    this.state = {
      username: '',
      description: '',
      duration: 0,
      date: new Date(),
      users: []
    }
    this.onChangeUsername = this.onChangeUsername.bind(this);
    this.onChangeDescription = this.onChangeDescription.bind(this);
    this.onChangeDuration = this.onChangeDuration.bind(this);
    this.onChangeDate = this.onChangeDate.bind(this);
    this.onSubmit = this.onSubmit.bind(this);
  }
.
.
```


Setup create exercise list methods

```js
/src/components/create-exercise.component.js

export default class EditExercise extends Component {
  constructor(props) {
.
.
  onChangeUsername(e) {
    this.setState({
      username: e.target.value
    })
  }

  onChangeDescription(e) {
    this.setState({
      description: e.target.value
    })
  }

  onChangeDuration(e) {
    this.setState({
      duration: e.target.value
    })
  }

  onChangeDate(date) {
    this.setState({
      date: date
    })
  }

  onSubmit(e) {
    e.preventDefault();

    const exercise = {
      username: this.state.username,
      description: this.state.description,
      duration: this.state.duration,
      date: this.state.date
    }

    console.log(exercise);

    axios.post('http://localhost:5000/exercises/update/' + this.props.match.params.id, exercise)
      .then(res => console.log(res.data));

    window.location = '/';
  }
.
.
```


Setup create exercise list lifecycle methods

```js
/src/components/create-exercise.component.js

export default class EditExercise extends Component {
.
.
  componentDidMount() {
    axios.get('http://localhost:5000/exercises/'+this.props.match.params.id)
      .then(response => {
        this.setState({
          username: response.data.username,
          description: response.data.description,
          duration: response.data.duration,
          date: new Date(response.data.date)
        })   
      })
      .catch(function (error) {
        console.log(error);
      })

    axios.get('http://localhost:5000/users/')
      .then(response => {
        if (response.data.length > 0) {
          this.setState({
            users: response.data.map(user => user.username),
          })
        }
      })
      .catch((error) => {
        console.log(error);
      })

  }
.
.
```


Install datepicker 

```
$ npm install react-datepicker

+ react-datepicker@3.1.3
added 36 packages from 28 contributors and audited 37 packages in 12.274s

15 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

```js
/src/components/edit-exercise.component.js

.
.
import DatePicker from 'react-datepicker';
import "react-datepicker/dist/react-datepicker.css";
.
.
```


Setup create exercise list HTML

```js
/src/components/create-exercise.component.js


export default class EditExercise extends Component {
.
.
  render() {
    return (
    <div>
      <h3>Edit Exercise Log</h3>
      <form onSubmit={this.onSubmit}>
        <div className="form-group"> 
          <label>Username: </label>
          <select ref="userInput"
              required
              className="form-control"
              value={this.state.username}
              onChange={this.onChangeUsername}>
              {
                this.state.users.map(function(user) {
                  return <option 
                    key={user}
                    value={user}>{user}
                    </option>;
                })
              }
          </select>
        </div>
        <div className="form-group"> 
          <label>Description: </label>
          <input  type="text"
              required
              className="form-control"
              value={this.state.description}
              onChange={this.onChangeDescription}
              />
        </div>
        <div className="form-group">
          <label>Duration (in minutes): </label>
          <input 
              type="text" 
              className="form-control"
              value={this.state.duration}
              onChange={this.onChangeDuration}
              />
        </div>
        <div className="form-group">
          <label>Date: </label>
          <div>
            <DatePicker
              selected={this.state.date}
              onChange={this.onChangeDate}
            />
          </div>
        </div>

        <div className="form-group">
          <input type="submit" value="Edit Exercise Log" className="btn btn-primary" />
        </div>
      </form>
    </div>
    )
  }
}
```



Setup create users constructor

```js
/src/components/create-user.component.js

.
.
export default class CreateUser extends Component {
  constructor(props) {
    super(props);

    this.onChangeUsername = this.onChangeUsername.bind(this);
    this.onSubmit = this.onSubmit.bind(this);

    this.state = {
      username: ''
    }
  }
.
.
```


Setup create users methods

```js
/src/components/create-user.component.js

.
.
export default class CreateUser extends Component {
.
.
  onChangeUsername(e) {
    this.setState({
      username: e.target.value
    })
  }

  onSubmit(e) {
    e.preventDefault();

    const user = {
      username: this.state.username
    }

    console.log(user);

    axios.post('http://localhost:5000/users/add', user)
      .then(res => console.log(res.data));

    this.setState({
      username: ''
    })
  }
.
.
```


Setup create users layout

```js
/src/components/create-user.component.js
/src/components/create-user.component.js

.
.
export default class CreateUser extends Component {
.
.
  render() {
    return (
      <div>
        <h3>Create New User</h3>
        <form onSubmit={this.onSubmit}>
          <div className="form-group"> 
            <label>Username: </label>
            <input  type="text"
                required
                className="form-control"
                value={this.state.username}
                onChange={this.onChangeUsername}
                />
          </div>
          <div className="form-group">
            <input type="submit" value="Create User" className="btn btn-primary" />
          </div>
        </form>
      </div>
    )
  }
}
.
.
```