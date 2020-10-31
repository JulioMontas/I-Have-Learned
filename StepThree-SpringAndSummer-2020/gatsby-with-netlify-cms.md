# Gatsby with Netlify CMS
Full Stack Severless   

Framework | APIs | CMS | 
---|---|---|
Gatsby JS | GraphQL | Netlify

## Step 1.0: Getting Started

Create a new app: 
```
$ gatsby new jm0001
```
Change into the directory and start the development mode:
```
$ cd jm0001 && gatsby develop
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

## Step 1.1: Integrate MDX

Install the `gatsby-plugin-mdx` plugin
```
npm install gatsby-plugin-mdx @mdx-js/mdx @mdx-js/react

For any problems run
```
npm install
```

Then add `gatsby-plugin-mdx` to your `gatsby-config.js` in the plugins section.
```
module.exports = {
  plugins: [`gatsby-plugin-mdx`]
}
```

## Step 1.2: Adding Markdown Pages
Install npm packages:

```
$ npm install --save gatsby-source-filesystem gatsby-transformer-remark
```

Open `gatsby-config.js` 
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
mkdir -p ./src/mdx-pages && touch ./src/mdx-pages/post-1.mdx
```

Open `post-1.mdx` to add and see the result in the terminal bundle build
```
---
title: "My first blog post"
description: "Hello World"
slug: "/blog/my-first-post"
date: "2021-01-24"
---
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



Making a new distanation to `/case-study/my-second-post` by adding a new markdown file
```
touch ./src/markdown-pages/post-2.md
```

Open `post-2.md` to add and see the result in the terminal bundle build:
```
---
slug: "/case-study/my-second-post"
date: "2020-09-18"
title: "My Second blog post"
description: "Hello World MDF"
---
Textttttt
```


Make the url for `/case-study/`:
```
touch ./src/pages/case-study.js
```

Inisde `case-study.js` add:
```
import React from "react"
import { graphql } from "gatsby"
import PostLink from "../components/post-link"

const CaseStudyPage = ({
  data: {
    allMarkdownRemark: { edges },
  },
}) => {
  const Posts = edges
    .filter(edge => !!edge.node.frontmatter.date) // You can filter your posts based on some criteria
    .map(edge => <PostLink key={edge.node.id} post={edge.node} />)

  return <div>{Posts}</div>
}

export default CaseStudyPage

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
            description
          }
        }
      }
    }
  }
`
```
## Step 1.5: Add MDX to an existing Gatsby site
Add `gatsby-plugin-mdx` and MDX as dependencies and also install `gatsby-plugin-feed-mdx` for our RSS feeds
```
npm install --save gatsby-plugin-mdx gatsby-plugin-feed-mdx @mdx-js/mdx @mdx-js/react

npm install gatsby-remark-images gatsby-plugin-sharp
npm install gatsby-remark-responsive-iframe
npm install gatsby-transformer-remark gatsby-remark-prismjs prismjs
npm install gatsby-remark-copy-linked-files
npm install gatsby-remark-smartypants
```


Update your `gatsby-config.js`, replace `gatsby-transformer-remark` with `gatsby-plugin-mdx`
```
{
      resolve: `gatsby-plugin-mdx`,
      options: {
        extensions: [".mdx", ".md"],
        gatsbyRemarkPlugins: [
          {
            resolve: `gatsby-remark-images`,
            options: {
              maxWidth: 590,
            },
          },
          {
            resolve: `gatsby-remark-responsive-iframe`,
            options: {
              wrapperStyle: `margin-bottom: 1.0725rem`,
            },
          },
          `gatsby-remark-prismjs`,
          `gatsby-remark-copy-linked-files`,
          `gatsby-remark-smartypants`,
        ],
      },
    },
```
Restart `gatsby develop` and add an `.mdx` page to `src/pages`

Remove `gatsby-transformer-remark` and `gatsby-plugin-feed`, you can uninstall them.
```
npm uninstall --save gatsby-transformer-remark gatsby-plugin-feed
```

In `gatsby-node.js`, replace `allMarkdownRemark` with `allMdx` and `MarkdownRemark` with `Mdx`.


In src/pages/index.js, replace allMarkdownRemark with allMdx in the render() method.

Also replace allMarkdownRemark with allMdx in the GraphQL query.

In src/templates/blog-post.js, replace markdownRemark with mdx in the render() method.
