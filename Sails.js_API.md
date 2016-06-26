#Getting Started with Sails.js v0.12.3

## Getting in less than 5min.
Install the latest stable release
```
npm -g install sails
```

Update your old version of Sail.js
```
npm update -g sails
```

Create a new app
```javascript
sails new newProject
```

Create a new app with out the Front-end
```javascript
sails new newProject --no-frontend
```

Open the app folder
```javascript
cd newProject
```

Create a model and a controller
```javascript
sails generate api user
```

Run Sails.js
```
sails lift
```

In the browser URL view or add the data or use use [Postman Chrome Extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)
```javascript
http://localhost:1337/user/create?name=Jose
```

`config/cors.js` allow CORS on all routes
```javascript
allRoutes: true
```

`config/blueprint.js` enabled by default, but should be disabled in production.
```javascript
shortcuts:false
```


## Using PostgreSQL

- Open PostgreSQL: `psql`

### PostgreSQL level
- Show all databases: `\l`
- Create database: `CREATE DATABASE sails_sails_db;`
- Connect to specific database: `\connect sails_sails_db`
- Drop database: `DROP DATABASE sails_sails_db;`

### Database level
- List all tables: `\d sails_sails_db`

### Installing sails-postgresql
Installing PostgreSQL as a dependencies
```
npm install --save sails-postgresql
```

`config/connections.js` Add PostgreSQL
```
julioPostgresqlServer: {
  adapter: 'sails-postgresql',
  host: 'localhost',
  user: 'null',
  password: '',
  database: 'sails_sails_db'
}
```

`config/models.js` replace `connection: 'localDiskDb',` to
```
connection: 'julioPostgresqlServer'
```

`api/models/User.js` not the best model but for testing is fine
```javascript
/**
 * User.js
 *
 * @description :: TODO: You might write a short summary of how this model works and what it represents here.
 * @docs        :: http://sailsjs.org/documentation/concepts/models-and-orm/models
 */

module.exports = {
  attributes: {
    bio:{
      type: 'string'
    },
    first_name:{
      type: 'string'
    },
    last_name:{
      type: 'string'
    },
    age:{
      type: 'date'
    },
    gender:{
      type: 'integer'
    },
    email:{
      type: 'array'
    },
    location:{
      type: 'string'
    },
    birthday:{
      type: 'date'
    },
    is_admin:{
      type: 'boolean'
    }
  }
};
```

Specifies the type of data that will be stored in this attribute [Attribute Options](http://sailsjs.org/documentation/concepts/models-and-orm/attributes)
```
string
text
integer
float
date
datetime
boolean
binary
array
json
mediumtext
longtext
objectid
```

## To render React.JS [Video with only 109 views](https://www.youtube.com/watch?v=SaGNKRKvB-c)


`config/views.js` to add this
```javascript
engine:{
  ext: 'jsx',
  fn: require('express-react-views').createEngine()
},
```

`views/homepage.ejs` rename to the file extension .jsx
```javascript
import React from 'react';

class HelloWorld extends React.Component {
  render() {
    return <p>Hello, world!</p>;
  }
}

export default HelloWorld;
```
