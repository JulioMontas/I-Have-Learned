# Gatsby with Netlify CMS
Full Stack Severless   

Framework | APIs | CMS | 
---|---|---|
Gatsby JS | GraphQL | Netlify

## Step 1.0: Getting Started

Create a new app: 
```
$ gatsby new hello-world
```
Change into the directory and start the development mode:
```
$ cd hello-world && gatsby develop
```

Visit the site locally and GraphiQL:
```
$ http://localhost:8000/
$ http://localhost:8000/___graphql
```

Inside GraphiQL make the querly:

```
query MyQuery {
  allSitePage {
    distinct(field: path)
  }
}
```
The query result:
```
{
  "data": {
    "allSitePage": {
      "distinct": [
        "/",
        "/404.html",
        "/404/",
        "/dev-404-page/",
        "/page-2/",
        "/using-typescript/"
      ]
    }
  },
  "extensions": {}
}
```
## Step 1.1: Integrate Netlify CMS
Install npm packages:
```
$ npm install --save netlify-cms-app gatsby-plugin-netlify-cms
```

Inside `gatsby-config.js`:
```
module.exports = {
  plugins: [`gatsby-plugin-netlify-cms`],
}
```
Create a `/admin/config.yml`:
```
mkdir -p ./static/admin
touch ./static/admin/config.yml
```
Inside `config.yml` add:
```
backend:
  name: test-repo

media_folder: static/img
public_folder: /img
publish_mode: editorial_workflow
collections:
  - name: 'blog'
    label: 'Blog'
    folder: 'content/blog'
    create: true
    slug: 'index'
    media_folder: ''
    public_folder: ''
    path: '{{title}}/index'
    editor:
      preview: true
    fields:
      - {label: "Language", name: "language", widget: "select", options: ["en", "es"]}
      - { label: 'Title', name: 'title', widget: 'string' }
      - { label: 'Publish Date', name: 'date', widget: 'datetime' }
      - { label: 'Description', name: 'description', widget: 'string' }
      - { label: 'Body', name: 'body', widget: 'markdown' }
  - name: 'portfolio'
    label: 'Portfolio'
    folder: 'content/portfolio'
    create: true
    slug: 'index'
    media_folder: ''
    public_folder: ''
    path: '{{title}}/index'
    editor:
      preview: true
    fields:
      - {label: "Language", name: "language", widget: "select", options: ["en", "es"]}
      - { label: 'Title', name: 'title', widget: 'string' }
      - { label: 'Publish Date', name: 'date', widget: 'datetime' }
      - { label: 'Description', name: 'description', widget: 'string' }
      - { label: 'Body', name: 'body', widget: 'markdown' }
  - label: "Pages"
    name: "pages"
    files:
      - label: "Home Page"
        name: "home"
        file: "content/home.yml"
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Intro, name: intro, widget: markdown}
          - label: Team
            name: team
            widget: list
            fields:
              - {label: Name, name: name, widget: string}
              - {label: Position, name: position, widget: string}
              - {label: Photo, name: photo, widget: image}
      - label: "About Page"
        name: "about"
        file: "content/about.yml"
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Intro, name: intro, widget: markdown}
          - label: Locations
            name: locations
            widget: list
            fields:
              - {label: Name, name: name, widget: string}
              - {label: Address, name: address, widget: string}

```
Start the development mode:
```
$ gatsby develop
```
Run locally Netlify CMS
```
$ http://localhost:60556/admin/
```

## Step 1.2: Authenticating with `GitHub` or `GitLab`:

GitHub
```
backend:
  name: github
  repo: your-username/your-repo-name
```

GitLab
```
{
  resolve: `gatsby-plugin-netlify-cms`,
  options: {
    enableIdentityWidget: false,
  }
}
```
## Step 1.3: Adding Markdown Pages
