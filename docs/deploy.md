# 部署文档
部署环境：Ubuntu16.04，容器：docker

## 前端自动集成和自动部署
前端采用了travisCI和Jenkins两套持续集成工具。在travisCI上会运行`yarn run lint`对代码进行静态检查。而Jenkins上进行自动部署。

### travis
travisCI相关的配置直接修改.travis.yml文件即可。如果构建失败，将会有邮件服务通知相关负责人员查看构建失败原因。（e.g. 如node的版本和某些依赖不兼容，在查看命令行输出之后可以修改node的版本，解决构建失败的问题）

同时，在readme中也放置了构建状态的徽章，如果徽章显示failed，即构建失败，需要前往查看构建失败的原因。

### Jenkins
jenkins是java编写的自动构建工具，Jenkins的部署和开启比较复杂，详情请参考Jenkins的部署文档。它可以通过GitHub的webhooks自动触发构建任务，更新本地代码，并将结果返回到GitHub上。

通过将新build生成的代码放入到nginx的docker 容器中，我们就可以完成自动部署，具体脚本如下：
```shell
yarn install --registry=https://registry.npm.taobao.org
yarn run lint
echo 'lint success'
docker exec youyu-fe rm -rf /www/test
docker cp dist/ youyu-fe:/www/test
docker cp default.conf youyu-fe:/etc/nginx/conf.d
docker exec youyu-fe nginx -t
docker exec youyu-fe service nginx reload
```
在此之前，我们还需要用docker 构建一个nginx镜像的容器。具体命令如下：
```shell
docker container run --name youyu-fe -d -p xxxx:yyyy -it nginx
```
由于服务器资源有限，我们没有选择在服务器上进行`vue-service-cli build`，而是在本地进行build，并将dist文件夹上传到GitHub上。



## 服务端部署文档

服务端使用 *travis* 和 *docker-compose* 实现自动部署

### travis

利用 ssh 免密登录 可以实现在 travis 上 ssh 进远程服务器运行脚本，配置过程不再赘述。

```yml
# travis.yml
language: go

go:
  - 1.12.x

addons:
  ssh_known_hosts:
    - "$server_ip"

# Go module on
env:
  - GO111MODULE=on

 # Decrypt public key
before_install:
  - openssl aes-256-cbc -K $encrypted_f33d63438573_key -iv $encrypted_f33d63438573_iv 
    -in .travis/id_rsa.enc -out ~/.ssh/id_rsa -d
  - chmod 600 ~/.ssh/id_rsa

# Lint & Build & Test
script:
  - go build -mod=vendor
  - diff -u <(echo -n) <(gofmt -d $(find . -type f -name "*.go" ! -path "./vendor/*"))
  - go test -v ./...

# Deployee
after_success:
  - test $TRAVIS_BRANCH = "master" && test $TRAVIS_PULL_REQUEST = "false" && ssh travis@$server_ip -o StrictHostKeyChecking=no "bash ~/Youyu-se/.travis/deploy.sh"
```



### Docker-compose

 docker-compose 的配置也很简单，主要是将 **构建** 和 **端口映射** 做好就行。

```yml
version: '3'
services:
  web:
    container_name: youyu-se
    image: golang:latest
    working_dir: /go/src/github.com/surplus-youyu/Youyu-se
    command: go run -mod=vendor main.go		# Build & Run
    env_file:
      - .env								# Secret config
    ports:
      - "8888:8080"							# Port mapping
    volumes:
      - ..:/go/src/github.com/surplus-youyu/Youyu-se
```



## 数据库部署文档
只需要开启mysql服务即可。
```
sudo systemctl start mysql
```
### 外网访问

- 将 `mysql.conf` 中的 `bind-address` 注释掉。
- 创建一个可以通过外网访问的用户：

```mysql
mysql> show grants for [secure];
+-----------------------------------------------------------------+
| Grants for [secure]@%                                           |
+-----------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO '[secure]'@'%' WITH GRANT OPTION |
+-----------------------------------------------------------------+
1 row in set (0.00 sec)
```

