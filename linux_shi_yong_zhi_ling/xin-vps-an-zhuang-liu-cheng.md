# 新 VPS 安裝流程

```bash

# Node.js

sudo curl -sL https://deb.nodesource.com/setup_12.x | bash -
sudo apt-get install -y nodejs
curl -L https://npmjs.org/install.sh | sudo sh
sudo chown -R $(whoami) ~/.npm
npm install yarn -g

# Docker

sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

# Docker compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Nginx

sudo apt update && sudo apt install nginx
sudo systemctl start nginx
vim /etc/nginx/sites-available/default

# SSL using cloudflare

1.Generate pem and key then add to nginx
https://support.cloudflare.com/hc/en-us/articles/115000479507

2.Add A record on cloudflare


# MongoDB
// docker 版可能會導致 ram 不夠
sudo apt-get install wget;

wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -;

echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

sudo apt-get update

sudo apt-get install -y mongodb-org

sudo systemctl start mongod
sudo systemctl status mongod
如果出現 failed status=14 參考此 https://askubuntu.com/a/1103513

mkdir ./db && mkdir ./log
mongod --dbpath ./db --logpath "./log/mongolog-${date}" &

db.createUser({ user: "admin", pwd: "jason2911", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })

// 使用 —auth 重新啟動

mongod --dbpath ./db --shutdown
mongod --dbpath ./db --logpath "./log/mongolog-${date}" --auth &
mongo  
use admin
db.auth('admin','密碼')  // 登入資料庫使用者

//創建資料庫與使用者
use sharehandi
db.createUser(
{
user: "yicheng",
pwd: "密碼",
roles: [ { role: "readWrite", db: "此資料庫名稱" }]
}
)

之後退出 mongo cli 然後重新進來 (因為db.auth兩次會產生 logical session error)

mongo;
use sharehandi;
db.auth('yicheng', '密碼');
db.sharehandi.insert({"name":"tutorials point"}); # 要新增資料後才算創建 DB

```

