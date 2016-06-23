#Getting Started with Sails.js v0.12

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
