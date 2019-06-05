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

## 数据库部署文档
只需要开启mysql服务即可。
```
sudo systemctl start mysql
```
如果外网无法访问这个数据库，可以在mysql.conf中修改bind-address，或者将其注释掉。