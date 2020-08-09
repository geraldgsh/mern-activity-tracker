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
