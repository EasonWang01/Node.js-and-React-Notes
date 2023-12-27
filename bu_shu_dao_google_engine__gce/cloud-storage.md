# Cloud Storage

創建 Bucket 公開存取

![](<../.gitbook/assets/截圖 2022-11-24 下午12.58.44.png>)

## 使用 API 上傳檔案

1.需要先新增服務帳務（Service accout ）與賦予角色權限

![](<../.gitbook/assets/截圖 2022-11-24 下午12.35.22.png>)

新增金鑰後存成 json

<div align="left">

<figure><img src="../.gitbook/assets/截圖 2023-12-27 下午5.41.03.png" alt="" width="375"><figcaption></figcaption></figure>

</div>

![](<../.gitbook/assets/截圖 2022-11-24 下午12.35.38.png>)

在 IAM 給予 storage 權限（如果點選下方服務帳戶沒辦法只接給予，其角色權限選單內容較少）

需要的話可以先去角色那邊創建特定角色權限

![](<../.gitbook/assets/截圖 2022-11-24 下午1.06.10.png>)

範例：

```javascript
const { Storage } = require("@google-cloud/storage");
function uploadFile(bucketName, filePath, destFileName) {
  return new Promise(async (resolve, reject) => {
    try {
      const storage = new Storage({ keyFilename: `${__dirname}/../服務帳戶.json` });

      const options = {
        destination: destFileName,
        preconditionOpts: { ifGenerationMatch: 0 },
      };

      await storage.bucket(bucketName).upload(filePath, options);
      resolve({
        success: true,
        bucketName,
        filePath,
      });
    } catch (error) {
      reject(error);
    }
  });
}
module.exports = {
  uploadFile,
};

```
