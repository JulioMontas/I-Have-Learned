# Gatsby Theme SaaS  

### Tutorial files
- [Video: Gatsby Theme Authoring](https://egghead.io/lessons/gatsby-set-up-yarn-workspaces-for-gatsby-theme-development)
- [Text: Setting Up Yarn Workspaces For Theme Development](https://www.gatsbyjs.com/blog/2019-05-22-setting-up-yarn-workspaces-for-theme-development/)
- [Text: Building A Theme](https://www.gatsbyjs.com/tutorial/building-a-theme/)

## 01 Start the Project
Create a new directory for the project
```bash
mkdir gatsby-theme-saas && cd gatsby-theme-saas
```

Create the `package.json`
```bash
touch package.json
```

Install `package.json` 
```bash
{
  "private": true,
  "workspaces": [
    "gatsby-theme-saas",
    "site"
  ]
}
```

Make a new folder for the theme directory
```bash
mkdir gatsby-theme-saas && cd gatsby-theme-saas && touch packpage.json index.js
```

Inside `gatsby-theme-saas/package.json` copy and paste or run `yarn init -y` to create a `package.json`
```bash
{
  "name": "gatsby-theme-saas",
  "version": "1.0.0",
  "licence": "MIT"
}

```

Make a new folder for the `site` directory
```bash
mkdir site && cd site && touch package.json
```

Inside `site/package.json` copy and paste 
```bash
{
  "private": true,
  "name": "site",
  "version": "1.0.0",
  "main": "gatsby-config.js",
  "license": "MIT",
  "scripts": {
    "build": "gatsby build",
    "develop": "gatsby develop",
    "clean": "gatsby clean"
  }
}
```


From the root directory, run to install dependencies
```bash
yarn workspace site add gatsby react react-dom
```

From the root directory, Then add the following as peer dependencies to the theme.
```bash
yarn workspace gatsby-theme-saas add --peer gatsby react react-dom
```

Start Gatsby  
```bash
yarn workspace site develop
```


Add a `gatsby-config.js` file to the theme directory.
```bash
module.exports = {
  plugins: [],
}
```


Site setup
```bash
yarn workspace site add gatsby-theme-saas@1.0.0
```



In the example site, create a gatsby-config.js file and add the theme.
```bash
module.exports = {
  plugins: ["gatsby-theme-example-workspaces"],
}
```


## 02 Connected the data, Install CMS

```bash
yarn workspace gatsby-theme-saas add gatsby-source-datocms
```

In your `gatsby-theme-saa/gatsby-config.js` update it to
```bash
const path = require("path")
module.exports = {
  plugins: [
    {
      resolve: "gatsby-plugin-page-creator",
      options: {
        path: path.join(__dirname, "src/pages"),
      },
    },
    {
      resolve: `gatsby-source-datocms`,
      options: {
        apiToken: apiToken: process.env.DATO_API_TOKEN,
        preview: false,
        disableLiveReload: false,
      },
    },
  ],
}
```

Tutorial files
- [DatoCMS](https://www.datocms.com/docs/gatsby/getting_started)


## 03 Make the pages.

