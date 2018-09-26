# Network

```js
    fetch(network.API_ENDPOINT, {
      method: 'POST',
      headers: {
        Accept: 'application/json',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        account: this.state.account,
        password: this.state.password,
      }),
    }).then((response) => response.json())
    .then((res) => {
      console.log(res)
    })
    .catch((error) => {
      console.error(error);
    });
```



