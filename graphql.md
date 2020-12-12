# GraphQL

## 與 RESTful、GRPC類似，均為 http 上層的後端與前端溝通方式

## Server 用法 

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

resolvers 用來處理當接收到 query 或 mutation 或 subscribe 時要做的對應處理，resolvers 包含三個預設參數 \(parent, args, context\)

context 讓 server 可以傳遞訊息讓 resolvers 第三個參數接收

## resolvers

parent 參數只能存取最多上一層，無法使用 `parent.parent`

args 可存取使用者在 client 下指令時 function 後的參數



