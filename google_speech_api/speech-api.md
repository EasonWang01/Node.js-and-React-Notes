# speech api

```text
var msg = new SpeechSynthesisUtterance();
```

```text
msg.text = 'Hello World';

var voices = window.speechSynthesis.getVoices();

msg.voice = voices[19]; //設定語言

speechSynthesis.speak(msg);
```

參考至  
[https://developers.google.com/web/updates/2014/01/Web-apps-that-talk-Introduction-to-the-Speech-Synthesis-API?hl=en](https://developers.google.com/web/updates/2014/01/Web-apps-that-talk-Introduction-to-the-Speech-Synthesis-API?hl=en)

