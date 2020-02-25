# OpenCV

[https://github.com/peterbraden/node-opencv](https://github.com/peterbraden/node-opencv)

安裝:

```text
sudo yarn cache clean
brew cleanup
brew install pkg-config
brew install opencv@2
brew link --force opencv@2

echo 'export PATH="/usr/local/opt/opencv@2/bin:$PATH"' >> ~/.zshrc
yarn add opencv
```

範例：

```javascript
const cv = require('opencv');

cv.readImage("./test.jpg", function (err, im) {
  im.detectObject(cv.FACE_CASCADE, {}, function (err, faces) {
    for (var i = 0; i < faces.length; i++) {
      var x = faces[i]
      im.ellipse(x.x + x.width / 2, x.y + x.height / 2, x.width / 2, x.height / 2);
    }
    im.save('./out.jpg');
  });
})
```

