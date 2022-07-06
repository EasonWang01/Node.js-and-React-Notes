---
description: 上傳檔案
---

# 上傳檔案

使用 multer 模組

```javascript
const multer = require("multer");

// Multer Upload Storage
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, `${__dirname}/../uploads/`); // 更改為上傳目的資料夾，資料夾須先創建
  },
  filename: (req, file, cb) => { // 上傳後的檔案名稱
    cb(null, file.fieldname + "-" + Date.now() + "-" + file.originalname);
  },
});
// Filter for CSV file
const csvFilter = (req, file, cb) => {
  if (file.mimetype.includes("csv")) { // 規定可上傳什麼檔案
    cb(null, true);
  } else {
    cb("Please upload only csv file.", false);
  }
};
const upload = multer({ storage: storage, fileFilter: csvFilter });

// upload.single("file") 對應到前端 input 的 name  <input type="file" name='file' />
router.post("/upload-csv", upload.single("file"), async (req, res) => {
  try {
    if (req.file == undefined) {
      return res.status(400).send({
        message: "Please upload a CSV file!",
      });
    }
    res.json({
      success: true,
    });
  } catch (err) {
    console.log(err);
  }
});
```
