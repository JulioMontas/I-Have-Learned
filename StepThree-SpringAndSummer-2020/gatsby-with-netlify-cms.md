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
Install npm packages:
```
$ npm install --save gatsby-source-filesystem
```

Open `gatsby-config.js` to add:
```
plugins: [
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      name: `markdown-pages`,
      path: `${__dirname}/src/markdown-pages`,
    },
  },
]
```
Install npm packages:
```
$ npm install --save gatsby-transformer-remark
```
Open `gatsby-config.js` to add:
```
plugins: [
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      path: `${__dirname}/src/markdown-pages`,
      name: `markdown-pages`,
    },
  },
  `gatsby-transformer-remark`,
]
```

Add a Markdown file
```
mkdir -p ./src/markdown-pages
touch ./src/markdown-pages/post-1.md
```

Open `post-1.md` to add and see the result in the terminal bundle buil
```
---
slug: "/blog/my-first-post"
date: "2019-05-04"
title: "My first blog post"
---
```

## Step 1.4: Create a page template for the Markdown files
Add a template file
```
mkdir -p ./src/templates
touch ./src/templates/blogTemplate.js
```

Open `blogTemplate.js` to add:
```
import React from "react"
import { graphql } from "gatsby"

export default function Template({
  data, // this prop will be injected by the GraphQL query below.
}) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post-container">
      <div className="blog-post">
        <h1>{frontmatter.title}</h1>
        <h2>{frontmatter.date}</h2>
        <div
          className="blog-post-content"
          dangerouslySetInnerHTML={{ __html: html }}
        />
      </div>
    </div>
  )
}

export const pageQuery = graphql`
  query($slug: String!) {
    markdownRemark(frontmatter: { slug: { eq: $slug } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        slug
        title
      }
    }
  }
`
```

Inside `gatsby-node.js` add:
```
exports.createPages = async ({ actions, graphql, reporter }) => {
  const { createPage } = actions

  const blogPostTemplate = require.resolve(`./src/templates/blogTemplate.js`)

  const result = await graphql(`
    {
      allMarkdownRemark(
        sort: { order: DESC, fields: [frontmatter___date] }
        limit: 1000
      ) {
        edges {
          node {
            frontmatter {
              slug
            }
          }
        }
      }
    }
  `)

  // Handle errors
  if (result.errors) {
    reporter.panicOnBuild(`Error while running GraphQL query.`)
    return
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.slug,
      component: blogPostTemplate,
      context: {
        // additional data can be passed via context
        slug: node.frontmatter.slug,
      },
    })
  })
}
```
Inside GraphiQL make the querly:

```
query MyQuery {
  allSitePage {
    distinct(field: path)
  }
}
```
If you see `/blog/my-first-post` query result we doing good
```
{
  "data": {
    "allSitePage": {
      "distinct": [
        "/",
        "/404.html",
        "/404/",
        "/blog/my-first-post",
        "/dev-404-page/",
        "/page-2/",
        "/using-typescript/"
      ]
    }
  },
  "extensions": {}
}
```


Make a new page `blog.js`
```
touch ./src/pages/blog.js
```

Inside `blog.js` add:

```
import React from "react"
import { graphql } from "gatsby"
import PostLink from "../components/post-link"

const BlogPage = ({
  data: {
    allMarkdownRemark: { edges },
  },
}) => {
  const Posts = edges
    .filter(edge => !!edge.node.frontmatter.date) // You can filter your posts based on some criteria
    .map(edge => <PostLink key={edge.node.id} post={edge.node} />)

  return <div>{Posts}</div>
}

export default BlogPage

export const pageQuery = graphql`
  query {
    allMarkdownRemark(sort: { order: DESC, fields: [frontmatter___date] }) {
      edges {
        node {
          id
          excerpt(pruneLength: 250)
          frontmatter {
            date(formatString: "MMMM DD, YYYY")
            slug
            title
          }
        }
      }
    }
  }
`
```

Create a new page `post-link.js`
```
touch ./src/components/post-link.js
```

Inside `post-link.js` add:
```
import React from "react"
import { Link } from "gatsby"

const PostLink = ({ post }) => (
  <div>
    <Link to={post.frontmatter.slug}>
      {post.frontmatter.title} ({post.frontmatter.date})
    </Link>
  </div>
)

export default PostLink
```
