# RefreshControl

下拉後可重新整理

{% embed url="https://reactnative.dev/docs/refreshcontrol" %}

### Native base 要放在 content 裡面

```javascript
  async handleOnRefresh() {
    this.fetchMail();
    this.setState({refreshing: true});
    this.setState({refreshing: false});
  }
          
            <Content
            refreshControl={
              <RefreshControl
                onRefresh={() => this.handleOnRefresh()}
                refreshing={this.state.refreshing}
                progressBackgroundColor={'#fff'}
              />
            }>
            ....
```

