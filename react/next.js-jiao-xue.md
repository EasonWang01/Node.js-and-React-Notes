# Next.js教學

## Next.js 教學

[https://nextjs.org/](https://nextjs.org/)

### 1.建立專案

1.開一個新資料夾，然後cd進去後輸入以下指令。

```
npm init -y
npm install --save react react-dom next
```

2.在package.json的script內加入以下

```
{
  "scripts": {
    "dev": "next"
  }
}
```

3\.

```
npm run dev
```

4.之後應該會顯示一個404畫面，這時我們新增一下pages 資料夾，裡面放入一個index.js，內容放入以下。

```javascript
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```

5.新增其他route時只要在pages內加入檔案即可。

> /pages/about.js

```javascript
export default () => (
  <div>
    <p>This is the about page</p>
  </div>
)
```

### 2.新增client Route

我們把index.js改為如下

```javascript
import Link from 'next/link'

const Index = () => (
  <div>
    <Link href="/about">
      <a>About Page</a>
    </Link>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```

> route的文件：[https://github.com/zeit/next.js#routing](https://github.com/zeit/next.js#routing)

### 3.新增共用layout component

[https://nextjs.org/learn/basics/using-shared-components/the-layout-component](https://nextjs.org/learn/basics/using-shared-components/the-layout-component)

## 4.引入\<script>或是其他html tag

```javascript
import Layout from '../components/MyLayout.js'
import Head from 'next/head'

export default () => (
    <Layout>
        <Head>
          <script src="https://checkout.stripe.com/v2/checkout.js"></script>
        </Head>
       <p>This is the about page</p>
    </Layout>
)
```

## 5.讀取url

> 加入`withRouter`
>
> 之後即可讀取到router的props

```javascript
import {withRouter} from 'next/router'
import Layout from '../components/MyLayout.js'

const Content = withRouter((props) => (
  <div>
    <h1>{props.router.query.title}</h1>
    <p>This is the blog post content.</p>
  </div>
))

const Page = (props) => (
    <Layout>
       <Content />
    </Layout>
)

export default Page
```

## 6.Router Masking

也就是更改實際的client URL

```javascript
    <Link as={`/p/${props.id}/scasc`} href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
```

> as 為顯示出來的，href為實際的router
>
> 但重新整理後會無法讀取該顯示出來的router

## 7.自訂Server

原先為使用Next.js內建的dev server，現在要來自己建一個Server

> 如果有不在page內路徑url，也必須要自訂server，否則重新整理會產生404頁面。

```javascript
const express = require('express')
const next = require('next')

const dev = process.env.NODE_ENV !== 'production'
const app = next({ dev })
const handle = app.getRequestHandler()

app.prepare()
.then(() => {
  const server = express()

  server.get('/p/:id', (req, res) => {
    const actualPage = '/post'
    const queryParams = { title: req.params.id } 
    app.render(req, res, actualPage, queryParams)
  })

  server.get('*', (req, res) => {
    return handle(req, res)
  })

  server.listen(3000, (err) => {
    if (err) throw err
    console.log('> Ready on http://localhost:3000')
  })
})
.catch((ex) => {
  console.error(ex.stack)
  process.exit(1)
})
```

[https://github.com/zeit/next.js#custom-server-and-routing](https://github.com/zeit/next.js#custom-server-and-routing)

## 8.使用Fetch獲取Data

把index.js改為如下

> 當client 跳轉到此頁面時會從client發出請求，但如果是重新整理頁面的話則是Server Side會發出請求並直接把資料渲染到頁面上。

```javascript
import Layout from '../components/MyLayout.js'
import Link from 'next/link'
import fetch from 'isomorphic-unfetch'

const Index = (props) => (
  <Layout>
    <h1>Batman TV Shows</h1>
    <ul>
      {props.shows.map(({show}) => (
        <li key={show.id}>
          <Link as={`/p/${show.id}`} href={`/post?id=${show.id}`}>
            <a>{show.name}</a>
          </Link>
        </li>
      ))}
    </ul>
  </Layout>
)

Index.getInitialProps = async function() {
  const res = await fetch('https://api.tvmaze.com/search/shows?q=batman')
  const data = await res.json()

  console.log(`Show data fetched. Count: ${data.length}`)

  return {
    shows: data
  }
}

export default Index
```

更新版:

> Next.js 9.3
>
> [https://nextjs.org/docs/api-reference/data-fetching/get-initial-props](https://nextjs.org/docs/api-reference/data-fetching/get-initial-props)

```javascript
export async function getStaticProps() {
  // Call an external API endpoint to get posts
  const res = await fetch(`${process.env.NEXT_PUBLIC_API_ENDPOINT}/}`)
  const data = await res.json()
  return {
    props: {
      data,
    },
  }
}

export default function Home({ data }) {
  ...
}
```

## 9. Style JSX

可以如下寫css

```jsx
export default () => (
  <Layout>
    <h1>My Blog</h1>
    <ul>
      {getPosts().map((post) => (
        <li key={post.id}>
          <Link as={`/p/${post.id}`} href={`/post?title=${post.title}`}>
            <a>{post.title}</a>
          </Link>
        </li>
      ))}
    </ul>
    <style jsx>{`
      h1, a {
        font-family: "Arial";
      }

      ul {
        padding: 0;
      }

      li {
        list-style: none;
        margin: 5px 0;
      }

      a {
        text-decoration: none;
        color: blue;
      }

      a:hover {
        opacity: 0.6;
      }
    `}</style>
  </Layout>
)
```

## 如果要加上外部css檔案

next.config.js

```javascript
const withCSS = require('@zeit/next-css')
module.exports = withCSS()
```

然後記得import './....css' 的檔案必須寫在pages/index.js中。

## 10. 部署

新增以下這兩行在 package.json 的 script 中。

```
"build": "next build",
"start": "next start -p 8000",
```

但如果想用自訂的Server可以在build完後使用

> 加上NODE\_ENV=production即可，自訂的Server為上面第七點的Server

```
"serve": "NODE_ENV=production node server.js"
```

## 使用Nginx

他跟create-react-app 不同之處在於他是server side render，所以沒有直接build然後用serve static的方式，而是需要啟動next的server然後用nginx做reverse proxy的方式。

```javascript
location / {
  # default port, could be changed if you use next with custom server
  proxy_pass http://localhost:3000;

  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection 'upgrade';
  proxy_set_header Host $host;
  proxy_cache_bypass $http_upgrade;

  # if you have try_files like this, remove it from our block
  # otherwise next app will not work properly
  # try_files $uri $uri/ =404;
}
```

**使用pm2**

> 到next專案下輸入如下：
>
> ```
> next build
> pm2 start npm --name "next" -- start
> ```

## 11. 部署到 Now

Now 為一個 [https://zeit.co/](https://zeit.co/) 所出的雲端快速建置服務，使用Heroku

安裝：

```
npm i -g --unsafe-perm now
```

之後再項目的根目錄輸入`now`

> 記得先到網站註冊帳號

部署後會看到如下（會給你分配一個 now.sh 的子網域）：

之後回到Zeit.co網站也可查看部署過的項目。

## 額外功能

#### 1. 輸出靜態頁面

新增 next.config.js

```javascript
module.exports = {
  exportPathMap: function () {
    return {
      '/': { page: '/' },
      '/about': { page: '/about' },
      '/p/hello-nextjs': { page: '/post', query: { title: "Hello Next.js" } },
      '/p/learn-nextjs': { page: '/post', query: { title: "Learn Next.js is awesome" } },
      '/p/deploy-nextjs': { page: '/post', query: { title: "Deploy apps with Zeit" } },
      '/p/exporting-pages': { page: '/post', query: { title: "Learn to Export HTML Pages" } }
    }
  }
}
```

> 上面的config檔案描述 route 該導向哪個檔案，以及要Render的資料。

之後輸入以下即可

```
next export
```

會產生一個out資料夾，裡面包含一個html檔案。

> 可以使用Serve模組測試
>
> ```
> npm install serve -g
> cd out
> serve -p 8080
> ```

如果想要部署到 Now上，進入 out 資料夾後輸入 `now` 即可。
