#Getting Started with React 15.1.0

## Step I: Installation

Make new directory
```
mkdir front_END
```

Inside the `front_END` directory
```
mkdir app
touch webpack.config.js .gitignore .babelrc
```

Make a package.json
```
npm init
```

Packages install globally
```
npm install -g babel webpack webpack-dev-server
```

Packages install --save
```
npm install axios react react-dom react-router --save
```

Packages install --save-dev
```
npm install babel-core babel-loader babel-preset-es2015 babel-preset-react babel-preset-react-hmre eslint eslint-config-airbnb eslint-plugin-react html-webpack-plugin react-hot-loader webpack webpack-dev-server --save-dev
```

Inside the `package.json`
```
"scripts": {
  "production": "webpack -p",
  "start": "webpack-dev-server --progress --inline --hot"
},
```

Inside the `app` directory
```
mkdir components config containers utils
touch index.html index.js
```

Inside the `components` directory create this JS files
```
touch Home.js Main.js Header.js Footer.js
```

Inside the `config` directory create this JS file
```
touch routes.js
```

Inside the `containers` directory create this JS file
```

```

Inside the `utils` directory create this JS file
```
touch ajaxHelpers.js
```

## Step II: Setting the Front-End files

Inside the `webpack.config.js`
```
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var HtmlWebpackPluginConfig = new HtmlWebpackPlugin({
  template: path.join(__dirname, '/app/index.html'),
  filename: 'index.html',
  inject: 'body'
})

module.exports = {
  devtool: 'eval',
  entry: [
    './app/index.js'
  ],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  plugins: [
    HtmlWebpackPluginConfig
  ],
  module: {
    loaders: [{
      test: /\.js$/,
      loaders: ['babel-loader'],
      include: path.join(__dirname, 'app')
    }]
  }
};
```

Inside the `.babelrc`
```
{
  "presets": [
    "react",
    "es2015",
    "react-hmre"
  ]
}
```

Inside the `.eslintrc`
```

```

Inside the `app/index.html`
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>React Front-End</title>
  </head>
  <body>
    <div id="app"></app>
  </body>
</html>
```

Inside the `index.js`
```
import React from 'react';
import ReactDOM from 'react-dom';
import routes from './config/routes';

ReactDOM.render(
  routes,
  document.getElementById('app')
);
```

Inside the `components/Home.js`
```
import React from 'react';
import {Link} from 'react-router';

const Home = React.createClass({

  render: function() {
    return (
      <div>
        <h1>Main Page</h1>
      </div>
    );
  }
});

export default Home;
```

Inside the `components/Main.js`
```
import React from 'react';
import Header from './Header';
import Footer from './Footer';

const Main = React.createClass({
  render: function(){
    return(
      <div>
        <Header />
        {this.props.children}
        <Footer />
      </div>
    )
  }
});

export default Main;
```

Inside the `components/Header.js`
```
import React from 'react';

const Header = React.createClass({
  render: function(){
    return(
      <div>
        <h1>Header</h1>
      </div>
    )
  }
});

export default Header;
```

Inside the `components/Footer.js`
```
import React from 'react';

const Footer = React.createClass({
  render: function(){
    return(
      <div>
        <h1>Footer</h1>
      </div>
    )
  }
});

export default Footer;
```

Inside the `config/routes.js`
```
import React from 'react';
import { Router, Route, IndexRoute, hashHistory} from 'react-router';

/* routing */
import Main from '../components/Main';
import Home from '../components/Home';
import Header from '../components/Header';
import Footer from '../components/Footer';

const routes = (
  <Router history={hashHistory}>
    <Route path='/' component={Main}>
      <IndexRoute component={Home} />
      <Route path='header' component={Header} />
      <Route path='footer' component={Footer} />
    </Route>
  </Router>

);

export default routes;
```

Inside the `containers/?.js`
```

```

Inside the `containers/ajaxHelpers.js`
```
import axios from 'axios';

const helpers = {
  getTacos: function(){
    // all of YOUR route urls need to be updated for production
    return axios.get('http://HEROKU_URL/api/user/');
  },
  getTaco: function(){
    return axios.get('http://HEROKU_URL/api/user/')
  },
  addTaco: function(taco){
    return axios.post('http://HEROKU_URL/api/user/');
  },
  deleteTaco: function(taco){
    return axios.delete('http://HEROKU_URL/api/user/');
  },
  updateTaco: function(taco){
    return axios.put('http://HEROKU_URL/api/user/');
  }

}

export default helpers;
```

## Step III: Run React
Make new directory
```
npm start
```
