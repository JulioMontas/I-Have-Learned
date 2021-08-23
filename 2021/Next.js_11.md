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

`/pages/_app.js`
 ```
import '../styles/globals.css'

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}

export default MyApp
 ```
`/pages/index.js`
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
`/styles/global.js`
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
`/styles/Home.module.css`
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
