## 在 docker 中安装 mongodb

- 拉取镜像

```shell
docker pull mongo:4.2
```

推荐使用 mongodb 的 4.2 和 4.4 版本，这两个版本较为稳定。

不推荐使用最新版，在最新 6 版本的 mongodb 中没有自带  MongoDB shell，一些命令无法使用，如 mongo 等。需要去手动安装 MongoDB shell。

- 启动 mongo 容器

```shell
docker run -itd --name mongo -p 27017:27017 mongo:4.2 --auth
```

`--auth` 表示默认开启用户验证，开启此参数后必须进行用户登录后才可以进行数据库操作。

由于我们开启了用户验证，但是还没有创建用户，所以需要创建用户。注意，创建全局用户需要在 admin 数据库中。

```shell
# 设置管理员用户密码需要切换到admin库
use admin
# 创建管理员
db.createUser({user:"brokyz",pwd:"123456",roles:["root"]})
# 查看当前库所有用户信息
show users
# 删除用户据
db.dropUser("brokyz")
```

之后在 mongodb 命令行外登录可以使用命令

```shell
mongo -u brokyz -p 123456
```

在 mongodb 中登录数据库后默认都会处于 test 库，可以在登陆时进行指定，登陆时默认在 admin 库

```shell
mongo admin -u brokyz -p 123456
```

在 mongodb 命令行内登录

```shell
# 需要提前进入用户对应的库
use admin
# 登录
db.auth("brokyz","123456")
# 查看当前用户拥有权限的数据库
show dbs
```

创建单个数据库权限用户

```shell
# 创建数据库 school
use school
# 查看当前数据库拥有的用户，由于刚创建所有没有用户
show users
# 创建属于该数据库的用户
db.createUser({user:"school",pwd:"123456",roles:["dbOwner"]})
```

在 mongodb 命令行外登录单个数据库用户

```shell
mongo -u school -p 123456 --authenticationDatabase=school
```

在 mongodb 命令行内登录单个用户数据库

```shell
use school
db.auth("school","123456")
```

