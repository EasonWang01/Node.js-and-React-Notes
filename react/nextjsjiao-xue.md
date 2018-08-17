# Next.js 教學

[https://nextjs.org/](https://nextjs.org/)

## 1.建立專案

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

3.

```
npm run dev
```

4.之後應該會顯示一個404畫面，這時我們新增一下pages 資料夾，裡面放入一個index.js，內容放入以下。

```js
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```

5.新增其他route時只要在pages內加入檔案即可。

> /pages/about.js

```js
export default () => (
  <div>
    <p>This is the about page</p>
  </div>
)
```

## 2.新增client Route

我們把index.js改為如下

```js
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

> route的文件：[https://github.com/zeit/next.js\#routing](https://github.com/zeit/next.js#routing)

## 3.新增共用layout component

[https://nextjs.org/learn/basics/using-shared-components/the-layout-component](https://nextjs.org/learn/basics/using-shared-components/the-layout-component)

# 4.引入&lt;script&gt;或是其他html tag

```js
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

# 5.讀取url

> 加入`withRouter`
>
> 之後即可讀取到router的props

```js
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



