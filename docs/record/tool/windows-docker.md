# Windows .net core Project to push Docker 

## 生成镜像

docker build -t aspnetcoredocker .

## 查看Docker所有镜像

docker images

## 运行镜像

docker run --name=aspnetcoredocker -p 7601:80 -d aspnetcoredocker

## 查看正在运行的容器

docker ps

## 地址栏输入

http://localhost:7601/

![展示](https://github.com/Justin1107-good/skill/blob/07f52ee371746cfb0081454cbcf0a1535d405488/base/DockerDemo/DockerDemo/wwwroot/images/docker-success-result.png)

## 下载包学习

