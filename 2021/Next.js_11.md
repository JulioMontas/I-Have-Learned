# Step 1.0: Getting Started

Installing Next.JS 11 
```
yarn create next-app
```

Run the development server
```bash
npm run dev
# or
yarn dev
```


# Step 1.1: Default view
```
├── .git
├── .next
├── node_modules
├── pages
│   ├── _app.js
│   └── index.js
├── public
│   ├── favicon.ico
│   └── vercel.svg
├── styles
│   ├── Home.module.css
│   └── globals.css
├── README.md
├── package.json
└── yarn.lock
```

`./pages/_app.js`
 ```
import '../styles/globals.css'

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}

export default MyApp
 ```
`./pages/index.js`
```
import Head from 'next/head'
import styles from '../styles/Home.module.css'

export default function Home() {
  return (
    <div className={styles.container}>
      <h1>Hello</h1>
    </div>
  )
}
```
`./styles/global.js`
```
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen,
    Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}

a {
  color: inherit;
  text-decoration: none;
}

* {
  box-sizing: border-box;
}
```
`./styles/Home.module.css`
```
.container {
  min-height: 100vh;
  padding: 0 0.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

@media (max-width: 600px) {

}
```
`./package.json`
```
{
  "name": "name",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "11.0.1",
    "react": "17.0.2",
    "react-dom": "17.0.2"
  }
}

```
# Step 2.0: Getting Started with Strapi.js

HTTP client
```
yarn add axios
```
Update `./pages/index.js`
```
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import axios from 'axios';

export default function Languages() {
  return (
    <div className={styles.container}>
      <h1>Hello</h1>
    </div>
  )
}

axios.get('http://localhost:1337/restaurant').then(response => {
  console.log(response);
});

```    
# Step 3.0: Connecting Next.JS to GraphQL
Setting Up Apollo Client
```
yarn add @apollo/client graphql
```

