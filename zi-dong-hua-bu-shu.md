# 自動化部屬

安裝 GitLab

```text
mkdir GitLab && cd GitLab
curl https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml -O
docker-compose up
```

> 如果出現 ERROR: manifest for sameersbn/gitlab:13.0.7 not found:
>
>  把 docker-compose.yml 的gitlab image 版本改為 
>
> > sameersbn/gitlab:latest

