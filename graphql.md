# GraphQL

## 與 RESTful、GRPC類似，均為 http 上層的後端與前端溝通方式

## Server 用法&#x20;

```javascript
const typeDefs = gql`
  """
  使用者
  """
  type User {
    "識別碼"
    id: ID
    "名字"
    name: String
    "年齡"
    age: Int
    "朋友"
    friends(a: Int): [User]
  }

  type Query {
    hello: String
    "取得當下使用者"
    me: User
    "取得所有使用者"
    users: [User]
  }

  type Mutation {
    "新增使用者"
    addUser(id: ID, name: String!, age: Int): User
  }
`;

const resolvers = {
  Query: {
    hello: () => "world",
    me: () => users[0],
    users: () => users,
  },
  Mutation: {
    addUser: (root, args, context) => {
      const { id, name, age } = args;
      // 新增 post
      users.push({
        id,
        name,
        age,
      });
      return users[3];
    },
  },
  User: {
    friends: (parent, args, context) => {
      const { friendIds } = parent;
      return users.filter((user) => friendIds.includes(user.id));
    },
  },
};

const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: () => {
    return {a: ""}
  }
});
```

typeDefs 用來定義 type，內建有 query, mutation, subscribe 三種 type。

resolvers 用來處理當接收到 query 或 mutation 或 subscribe 時要做的對應處理，resolvers 包含三個預設參數 (parent, args, context)

context 讓 server 可以傳遞訊息讓 resolvers 第三個參數接收

## Query

```javascript
  query user{
    user {
      id
      name
      age
    }
  }
  
  等同於
  
    user {
      id
      name
      age
    }
```

{% embed url="https://stackoverflow.com/a/34185655/4622645" %}

React 範例：

> 記得要寫在 ApolloProvider 裡面的 component 內，如果直接在 ApolloProvider 同個 component 使用 useQuery 會無法

```javascript
import React from "react";
import { useQuery, gql } from "@apollo/client";

const getUsersQuery = gql`
  {
    users {
      id
      name
      age
    }
  }
`;

const Users = () => {
  const { loading, error, data } = useQuery(getUsersQuery);
  return <div>{data && data.users[0].name}</div>;
};

export default Users;
```

{% embed url="https://www.apollographql.com/docs/react/get-started/" %}

### Fragment

把 client 寫 query 時的結構拆出來，讓他可以重複使用

```javascript
const getUsersQuery = gql`
  {
    users {
      id
      name
      age
    }
  }
`;


// 等同於

const getUsersQuery = gql`
  {
    users {
      ...userInfo
      age
    }
  }
  fragment userInfo on User {
    id
    name
  }
`;
```

### Mutation

{% embed url="https://www.apollographql.com/docs/react/data/mutations/" %}

example:&#x20;

```javascript
import React from "react";
import { useQuery, useMutation, gql } from "@apollo/client";

const getUsersQuery = gql`
  {
    users {
      id
      name
      age
    }
  }
`;

// $ 為變數用法，useMutation 可在 option 傳入 variable
const mutationUsers = gql`
  mutation AddUser($id: ID!) {
    addUser(id: $id, name: "test", age: 15) {
      id
      name
      age
    }
  }
`;

const Users = () => {
  const { loading, error, data, refetch } = useQuery(getUsersQuery);
  const [addUser, { data: _data }] = useMutation(mutationUsers, {
    variables: {
      id: (Math.floor(Math.random() * 100)).toString(),
    },
  });
  
  const handleAddUser = () => {
    addUser();
    refetch(); // 再 query 一次新資料
  };
  return (
    <div>
      <button onClick={handleAddUser}>Add</button>
      {data && data.users.map((user, idx) => <div key={idx}>{user.name}</div>)}
    </div>
  );
};
export default Users;

```

### Subscription

Server: [https://www.apollographql.com/docs/apollo-server/data/subscriptions/](https://www.apollographql.com/docs/apollo-server/data/subscriptions/)

React: [https://www.apollographql.com/docs/react/data/subscriptions/](https://www.apollographql.com/docs/react/data/subscriptions/)

## typeDefs

1. 回傳 array 就用 \[] 把其他 type 包住
2. 直接在名稱上方可寫註解
3. ! 代表必填參數 `post(id: ID!): Post`

#### 預設 types

![](<.gitbook/assets/截圖 2020-12-12 下午2.54.54.png>)

{% embed url="https://graphql.org/learn/schema/" %}

#### 自定義 scalar type

> 也是寫在 resolver 然後使用 `new GraphQLScalarType()`

[https://ithelp.ithome.com.tw/articles/10206366](https://ithelp.ithome.com.tw/articles/10206366)

## resolvers

parent: 使用參數可以存取client 下指令時的function 上一層資料，但只能存取最多上一層，無法使用 `parent.parent`

args: 可存取使用者在 client 下指令時 function 後的參數

context: 存取傳進 Apollo Server 初始化時 context 的資料

> 還有第四個參數叫 info 但因為不常用，可以查看：
>
> [https://www.prisma.io/blog/graphql-server-basics-demystifying-the-info-argument-in-graphql-resolvers-6f26249f613a](https://www.prisma.io/blog/graphql-server-basics-demystifying-the-info-argument-in-graphql-resolvers-6f26249f613a)

### Subscribe

> 1.很重要一點是必須要把 client 端 ws link 與 http link 分開，不然會沒有資料回傳給 subscribe

> 2.server 的 resolver 一定要放以下格式，包含 Subscription, subscribe 與 asyncIterator
>
> ```javascript
> Subscription: {
>  .... : {
>    subscribe: pubsub.asyncIterator(...)
>  }
> } 
> ```

> 3\. server resolver  mutation 裡面記得要放
>
> ```
>       pubsub.publish(..., {
>         ...
>       });
> ```

完整範例：[https://gist.github.com/EasonWang01/4c1a803b5680bfc76b5c71756ebc6df4](https://gist.github.com/EasonWang01/4c1a803b5680bfc76b5c71756ebc6df4)

## Authorization

[https://ithelp.ithome.com.tw/articles/10205426](https://ithelp.ithome.com.tw/articles/10205426)

## 其他工具

#### [Prisma](https://www.prisma.io)

GraphQL 後端專用的 ORM，方便資料庫溝通

![](<.gitbook/assets/截圖 2020-12-12 下午2.46.31.png>)

#### Graphql-tag

parse gql string to AST ( 在 apollo-boost 內也有 )

{% embed url="https://github.com/apollographql/graphql-tag" %}

#### @apollo/client

取代了 react-apollo, apollo-boost 等，將一些 client 要用到的在 Apollo Client 3.0 都包含了進去。

[https://www.apollographql.com/docs/react/migrating/apollo-client-3-migration/](https://www.apollographql.com/docs/react/migrating/apollo-client-3-migration/)

#### React hook

{% embed url="https://www.apollographql.com/docs/react/development-testing/static-typing/" %}

## 常見問題

1.執行 query 後出現 unexpect token <&#x20;

> 通常是 endpoint 後的 url 路徑不對

[https://github.com/graphql-boilerplates/node-graphql-server/issues/274#issuecomment-381910134](https://github.com/graphql-boilerplates/node-graphql-server/issues/274#issuecomment-381910134)
