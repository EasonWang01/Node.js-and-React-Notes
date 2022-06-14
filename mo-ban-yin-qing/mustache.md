# Mustache

{% embed url="https://github.com/janl/mustache.js" %}

推薦可使用 CLI 工具動態產生檔案。

## 模板生成範例：

假設有多個不同的環境配置都用同一個模板來生成 yaml，可用如下

custom.json

```javascript
{
  "network": "mainnet",
  "factoryAddress": "0x0000",
}
```

the.template.yaml (共用模板)

```yaml
specVersion: 0.0.2
dataSources:
  address: {{factoryAddress}}
  network: {{network}}
```

輸入以下即可

```
npx mustache ./config/custom.json the.template.yaml > the.yaml
```

之後會生成 the.yaml 檔案內容如下

```yaml
specVersion: 0.0.2
dataSources:
  address: mainnet
  network: 0x0000
```
