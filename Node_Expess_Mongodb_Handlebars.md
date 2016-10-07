## Getting Started with Node.js, Express.js, MongoDB, Handlebars.

Install | View Engine | CSS Preprocessor | Database | Build tool
---|---|---|---|---
MVC | Handlebars | Node-Sass | MongoDB | Grunt

## Step I: Getting Started

### Install globally
```
$ brew install node
$ brew install mongodb
$ npm install -g express
$ npm install -g express-generator
$ npm install -g node-sass
```

Create the app files
```
$ express nodeapp --hbs --css sass --git

   create : nodeapp
   create : nodeapp/package.json
   create : nodeapp/app.js
   create : nodeapp/.gitignore
   create : nodeapp/public
   create : nodeapp/public/javascripts
   create : nodeapp/public/images
   create : nodeapp/public/stylesheets
   create : nodeapp/public/stylesheets/style.sass
   create : nodeapp/routes
   create : nodeapp/routes/index.js
   create : nodeapp/routes/users.js
   create : nodeapp/views
   create : nodeapp/views/index.hbs
   create : nodeapp/views/layout.hbs
   create : nodeapp/views/error.hbs
   create : nodeapp/bin
   create : nodeapp/bin/www

   install dependencies:
     $ cd nodeapp && npm install

   run the app:
     $ DEBUG=nodeapp:* npm start
```

Enter the directory
```
$ cd nodeapp
```

Add two more dependencies
```
$ npm install --save mongoose method-override
```

Install dependencies
```
$ npm install
```

### MVC folder style
From:
```
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       ├── style.css
│       ├── style.css.map
│       └── style.sass
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.hbs
    ├── index.hbs
    └── layout.hbs

7 directories, 11 files    
```
To:
```
.
├── app
│   ├── controllers
│   │   ├── api.js
│   │   ├── index.js
│   │   ├── languages.js
│   │   └── users.js
│   ├── models
│   │   ├── db.js
│   │   ├── languages.js
│   │   └── users.js
│   └── views
│       ├── error.hbs
│       ├── index.hbs
│       ├── languages
│       │   ├── edit.hbs
│       │   ├── index.hbs
│       │   ├── new.hbs
│       │   └── show.hbs
│       └── layout.hbs
├── app.js
├── bin
│   └── www
├── package.json
└── public
    ├── images
    ├── javascripts
    └── stylesheets
        └── style.css
10 directories, 18 files        
```

#### Lets get started, create 4 new directories in the root folder
```
$ mkdir -p app/{controllers,models,views}

└── app
    ├── controllers
    └── models
```

Move `routes/index.js` to the controllers
```
$ mv routes/index.js app/controllers
```

Delete `routes/users.js`
```
$ rm -rf routes
```

Inside `app.js` change `var routes = require('./routes/index');` to
```javascript
var controllers = require('./app/controllers/index');
```

Inside `app.js` change `app.use('/', routes);` to
```
app.use('/', controllers);
```

Move the `views` directorie to `app/`
```
mv views/ app/
```

Inside `app.js` change `app.set('views', path.join(__dirname, 'views'));` to
```javascript
app.set('views', path.join(__dirname, 'app/views'));
```

Inside `app/models` create a new file called `db.js`
```
touch app/models/db.js
```

Edit `db.js` with
```javascript
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/nodeapp');
```

Inside `app.js` add
```javascript
var db = require('./app/models/db');
```

Make a Database
```
$ mongod --dbpath=/nodeapp
```

Run the Database
```
$ sudo mongod
```

Check for any errors, http://localhost:3000/
```
$ npm start
```

#### Make the Model and Schema

Inside `app/models` create a new file called `times.js`
```
touch app/models/times.js
```

Inside `times.js` add
```javascript
var mongoose = require('mongoose');
var timeSchema = new mongoose.Schema({
  client_name: String,
  client_company: String,
  client_position: String,
  project_summary: String,
  price: Number,
  task_list: [String],
  done_with_client:  Boolean,
  created_at: { type: Date, default: Date.now },
  finish_at: { type: Date, default: Date.now }
});

mongoose.model('Time', timeSchema);
```

Inside `app.js` add
```javascript
var times = require('./app/models/times');
```

Inside `app/controllers` create a new file called `time.js`
```
touch app/controllers/time.js
```

Inside `time.js` add
```javascript
var express = require('express'),
    router = express.Router(),
    mongoose = require('mongoose'), //mongo connection
    bodyParser = require('body-parser'), //parses information from POST
    methodOverride = require('method-override'); //used to manipulate POST


// copied from method override page
router.use(bodyParser.urlencoded({ extended: true }));

router.use(methodOverride(function(req, res){
      if (req.body && typeof req.body === 'object' && '_method' in req.body) {
        // look in urlencoded POST bodies and delete it
        var method = req.body._method
        delete req.body._method
        return method
      }
}))

//build the REST operations at the base for times
//this will be accessible from http://127.0.0.1:3000/times if the default route for / is left unchanged
router.route('/')
    //GET all times
    .get(function(req, res, next) {
        //retrieve all times from Monogo
        mongoose.model('Time').find({}, function (err, times) {
              if (err) {
                  return console.error(err);
              } else {
                  //respond to both HTML and JSON. JSON responses require 'Accept: application/json;' in the Request Header
                  res.format({
                      //HTML response will render the index.jade file in the views/times folder. We are also setting "times" to be an accessible variable in our ejs view
                    html: function(){
                        res.render('times/index', {
                              title: 'All my times',
                              "times" : times
                          });
                    },
                    //JSON response will show all times in JSON format
                    json: function(){
                        res.json(times);
                    }
                });
              }
        });
    })
    //POST a new time
    .post(function(req, res) {
        // Get values from POST request. These can be done through forms or REST calls. These rely on the "name" attributes for forms
        var client_name = req.body.client_name;
        var client_company = req.body.client_company;
        var client_position = req.body.client_position;
        var project_summary = req.body.project_summary;
        var price = req.body.price;
        var task_list = req.body.task_list;
        var done_with_client = req.body.done_with_client;
        var created_at = req.body.created_at;
        var finish_at = req.body.finish_at;
        //call the create function for our database
        mongoose.model('Time').create({
            client_name: client_name,
            client_company: client_company,
            client_position: client_position,
            project_summary: project_summary,
            price: price,
            task_list: task_list,
            done_with_client: done_with_client,
            created_at: created_at,
            finish_at: finish_at
        }, function (err, time) {
              if (err) {
                  res.send("There was a problem adding the information to the database.");
              } else {
                  //time has been created
                  console.log('POST creating new time: ' + time);
                  res.format({
                      //HTML response will set the location and redirect back to the home page. You could also create a 'success' page if that's your thing
                    html: function(){
                        // If it worked, set the header so the address bar doesn't still say /adduser
                        res.location("times");
                        // And forward to success page
                        res.redirect("/times");
                    },
                    //JSON response will show the newly created time
                    json: function(){
                        res.json(time);
                    }
                });
              }
        })
    });

/* GET New time page. */
router.get('/new', function(req, res) {
    res.render('times/new', { title: 'Add New time' });
});

// route middleware to validate :id
router.param('id', function(req, res, next, id) {
    //console.log('validating ' + id + ' exists');
    //find the ID in the Database
    mongoose.model('Time').findById(id, function (err, time) {
        //if it isn't found, we are going to repond with 404
        if (err) {
            console.log(id + ' was not found');
            res.status(404)
            var err = new Error('Not Found');
            err.status = 404;
            res.format({
                html: function(){
                    next(err);
                 },
                json: function(){
                       res.json({message : err.status  + ' ' + err});
                 }
            });
        //if it is found we continue on
        } else {
            //uncomment this next line if you want to see every JSON document response for every GET/PUT/DELETE call
            //console.log(time);
            // once validation is done save the new item in the req
            req.id = id;
            // go to the next thing
            next();
        }
    });
});

router.route('/:id')
  .get(function(req, res) {
    mongoose.model('Time').findById(req.id, function (err, time) {
      if (err) {
        console.log('GET Error: There was a problem retrieving: ' + err);
      } else {
        console.log('GET Retrieving ID: ' + time._id);
        var timedob = time.dob.toISOString();
        timedob = timedob.substring(0, timedob.indexOf('T'))
        res.format({
          html: function(){
              res.render('times/show', {
                "timedob" : timedob,
                "time" : time
              });
          },
          json: function(){
              res.json(time);
          }
        });
      }
    });
  });

//GET the individual time by Mongo ID
router.get('/:id/edit', function(req, res) {
    //search for the time within Mongo
    mongoose.model('Time').findById(req.id, function (err, time) {
        if (err) {
            console.log('GET Error: There was a problem retrieving: ' + err);
        } else {
            //Return the time
            console.log('GET Retrieving ID: ' + time._id);
            //format the date properly for the value to show correctly in our edit form
          var timedob = time.dob.toISOString();
          timedob = timedob.substring(0, timedob.indexOf('T'))
            res.format({
                //HTML response will render the 'edit.hbs' template
                html: function(){
                       res.render('times/edit', {
                          title: 'time' + time._id,
                        "timedob" : timedob,
                          "time" : time
                      });
                 },
                 //JSON response will return the JSON output
                json: function(){
                       res.json(time);
                 }
            });
        }
    });
});

//PUT to update a time by ID
router.put('/:id/edit', function(req, res) {
    // Get our REST or form values. These rely on the "name" attributes
    var client_name = req.body.client_name;
    var client_company = req.body.client_company;
    var client_position = req.body.client_position;
    var project_summary = req.body.project_summary;
    var price = req.body.price;
    var task_list = req.body.task_list;
    var done_with_client = req.body.done_with_client;
    var created_at = req.body.created_at;
    var finish_at = req.body.finish_at;
   //find the document by ID
        mongoose.model('Time').findById(req.id, function (err, time) {
            //update it
            time.update({
              client_name: client_name,
              client_company: client_company,
              client_position: client_position,
              project_summary: project_summary,
              price: price,
              task_list: task_list,
              done_with_client: done_with_client,
              created_at: created_at,
              finish_at: finish_at
            }, function (err, timeID) {
              if (err) {
                  res.send("There was a problem updating the information to the database: " + err);
              }
              else {
                      //HTML responds by going back to the page or you can be fancy and create a new view that shows a success page.
                      res.format({
                          html: function(){
                               res.redirect("/times/" + time._id);
                         },
                         //JSON responds showing the updated values
                        json: function(){
                               res.json(time);
                         }
                      });
               }
            })
        });
});

//DELETE a time by ID
router.delete('/:id/edit', function (req, res){
    //find time by ID
    mongoose.model('Time').findById(req.id, function (err, time) {
        if (err) {
            return console.error(err);
        } else {
            //remove it from Mongo
            time.remove(function (err, time) {
                if (err) {
                    return console.error(err);
                } else {
                    //Returning success messages saying it was deleted
                    console.log('DELETE removing ID: ' + time._id);
                    res.format({
                        //HTML returns us back to the main page, or you can create a success page
                          html: function(){
                               res.redirect("/times");
                         },
                         //JSON returns the item with the message that is has been deleted
                        json: function(){
                               res.json({
                                 message : 'deleted',
                                 item : time
                               });
                         }
                      });
                }
            });
        }
    });
});

module.exports = router;
```

Inside `app.js` add
```javascript
var times = require('./app/controllers/times');
app.use('/times', times);
```

Inside directory `views` add the new folder `times` for the View
```
mkdir app/views/times && cd app/views/times
```

Inside `views` folder add 4 new files
```
touch index.hbs edit.hbs new.hbs show.hbs
```

## Step II: Make the V for View

#### Inside `index.hbs`
```handlebars

```

#### Inside `edit.hbs`
```handlebars

```

#### Inside `new.hbs`
```handlebars

```

#### Inside `show.hbs`
```handlebars

```


## Step III: Talk to the Database

JSON API Documentation

##### Users
```
GET      /api/v1/users       Get all users
GET      /api/v1/users/:id   Get user
POST     /api/v1/users       Create user
PUT      /api/v1/users/:id   Update user
DELETE   /api/v1/users/:id   Delete user
```

##### Languages
```
GET      /times       Get all languages
GET      /times/:id   Get language
POST     /times       Create language
PUT      /times/:id   Update language
DELETE   /times/:id   Delete language
```
