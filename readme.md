#pokemonNode

##项目描述
该项目用于制作口袋妖怪增删改查后台，可配合pc和vue项目。该项目有mysql增删改查封装的方法，以及多个异步的方式封装，和对路由的使用方法。

##运行
###git初始化
1.创建配置文件

```
npm init -y
```

2.全局配置git

```
git config --global user.name "名字" 回车
git config --global user.email "邮箱名" 回车
git config --list #查看加入信息
```

3.创建.gitigonre文件

```
# compiled output
/dist
/node_modules

# logs
logs
*.logs
npm-debug.log*

# os
.DS_Store

# IDE - VSCode
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json
```


###拷贝的package.json
1.拷贝之前项目的package.json 对照 安装依赖

```
npm install
```

2.nodemon运行启动文件
在package.json 找到启动文件

```
  "scripts": {
    "start": "node ./bin/www"
  },
```
nodemon之前全局安装，因此可以直接运行，之后访问正确的端口号http://localhost:3000/

```
nodenode ./bin/www
```

###初次git提交
1.创建远程仓库
> pokermon-express 

2.创建远程连接  
前提是可以配置ssh，并且提交

```
git remote add origin git@github.com:zyn-web/pokermon-express.git
git push origin master
```

##增加功能目录
###原始目录
配置完git和安装依赖之后的目录结构：

```
.
|____bin
| |____www
|____node_modules
|____public
| |____images
| |____javascripts
| |____stylesheets
| | |____style.css
|____.gitignore
|____package-lock.json
|____package.json
|____.git
|____views
| |____error.jade
| |____index.jade
| |____layout.jade
|____routes
| |____users.js
| |____index.js
|____app.js
```

###增加处理逻辑目录

```
.
|____util                   工具
| |____mysql                mysql处理
| | |____index.js           连接池增、删、改、查逻辑处理
| | |____config.js          连接mysql配置
|____bin
| |____www                  启动文件
|____node_modules           依赖
|____public                 静态资源
| |____images
| |____javascripts
| |____stylesheets
| | |____style.css
|____.gitignore             git push 配置
|____package-lock.json      在pacakge.json中间标出自己项目对npm各库包的依赖
|____package.json           项目配置文件
|____api                    
| |____poker                项目交互
| | |____index.js           交互路由容器
| | |____tool               增删改查
| | | |____edit.js          修改
| | | |____del.js           删除
| | | |____query.js         查询
| | | |____add.js           增加
|____views                  视图
| |____error.jade
| |____index.jade
| |____layout.jade
|____routes                 路由配置文件
| |____users.js
| |____index.js
|____app.js                 应用核心配置文件
```

##应用核心配置文件app.js
###不需要的内容
1.jade模版

```
// view engine setup  不需要模版
// app.set('views', path.join(__dirname, 'views'));
// app.set('view engine', 'jade');
```

2.两个路由

```
//模版自己创建的这两个可以不用
// app.use('/', indexRouter);
// app.use('/users', usersRouter);
```

###需要的内容
1.设置静态文件目录   
指向的是views，也就是说访问views 这一级就是 http://localhost:3000

```
app.use(express.static(path.join(__dirname, 'views')));
```

2.定义多个目录下文件为静态资源文件     
可以定义多个目录下文件为静态资源文件，服务器会根据你定义的顺序匹配
注意： 先定义先匹配  而不是 后面的定义覆盖前面的定义
可以在前面加一个虚拟路径，来加载静态资源文件

```
app.use('/public', express.static(path.join(__dirname + '/public')))
```

3.配置跨域

```
app.all('*', function (req, res, next) {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  res.header('Access-Control-Allow-Methods', '*');
  res.header('Content-Type', 'application/json;charset=utf-8');
  next();
});
```

##引用关系
所有用于逻辑的文件相互的引入关系

```
util/mysql/config.js             暴露a
util/mysql/index.js              引入a 暴露b

api/poker/tool                   引入b  暴露c
api/poker/index.js               引入c 暴露d 

app.js                           引入d 暴露e

./bin/www                        引入e
```

##接口

查询： 
[http://localhost:3000/poker/query](http://localhost:3000/poker/query)    
增加： 
[http://localhost:3000/poker/add](http://localhost:3000/poker/add)   
删除： 
[http://localhost:3000/poker/del](http://localhost:3000/poker/del)   
修改： 
[http://localhost:3000/poker/edit](http://localhost:3000/poker/edit)
