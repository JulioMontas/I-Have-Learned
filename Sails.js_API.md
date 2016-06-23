#Getting Started with Sails.js v0.12


Install the latest stable release
```javascript
 sudo npm -g install sails
```

Create a new app:
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
```javascript
sails lift
```

Inside the URL add data to the API of user [Postman Chrome Extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)
```javascript
http://localhost:1337/user/create?name=Jose
```

To disable the URL CRUD when you finish production.
```javascript
Go over to the ‘blueprints-js file’ and change /shortcuts/ to false
```
