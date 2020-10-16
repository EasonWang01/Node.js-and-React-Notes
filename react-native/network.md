# Network

範例

```javascript
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

> 如果出現Request error，通常是你使用模擬器
>
> 改為如下即可
>
> Genymotion
>
> ```text
> http://10.0.3.2
> ```
>
> Android studio emulator
>
> ```text
> http://10.0.2.2
> ```
>
> [https://stackoverflow.com/questions/5528850/how-do-you-connect-localhost-in-the-android-emulator](https://stackoverflow.com/questions/5528850/how-do-you-connect-localhost-in-the-android-emulator)

