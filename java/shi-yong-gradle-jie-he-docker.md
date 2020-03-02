# 使用gradle結合docker



## 安裝

安裝Gradle

[https://gradle.org/install/](https://gradle.org/install/)

安裝Docker

[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)

下載範例專案

```text
git clone https://github.com/codedata/JavaTutorial/tree/master/exercises/exercise10/BasicWeb
cd /JavaTutorial/exercises/exercise10/BasicWeb
```

## 打包成war

在build.gradle同層路徑輸入 :

```text
gradle tomcatRunWar
```

## 把war檔放入Docker執行

[http://codeomitted.com/deploy-war-file-to-docker-image/](http://codeomitted.com/deploy-war-file-to-docker-image/)

新增檔案名為 : Dockerfile

```text
# Pull base image
From tomcat:8-jre8

# Maintainer
MAINTAINER "xxx <xxx@gmail.com">

# Copy to images tomcat path
ADD BasicWeb.war /usr/local/tomcat/webapps/
```

然後build

```text
docker build -t webserver .
```

執行

```text
docker run -it --rm -p 8080:8080 --name dockerwar webserver
```

