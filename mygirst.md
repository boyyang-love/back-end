# querystring 
## qs.parse(str,'#','-')
   ```
const qs = require('querystring');
let str = 'name-boyyang#age-18';
let obj = qs.parse(str);
console.log(obj);
   ```
+ parse中第一个参数是以哪一个符号来拆分键值对，第二个参数是以哪一个符号来拆分键，值
## stringfy(str, '#' ,'-')
```
const qs = require('querystring');
let obj = {name: 'boyyang', age: 18};
let str = qs.stringify(obj);
console.log(str);
```
+第一个参数是以什么符号来切分键值对，第二个是用什么符号来切分键，值
## escape(编码)  unescape(解码)
+ 编码
```
const qs = require('querystring');
let string = 'hialj你hoallf号';
let result = qs.escape(string);
console.log(result);
```
+ 解码
```
const qs = require('querystring');
let escape = 'hialj%E4%BD%A0hoallf%E5%8F%B7';
let result = qs.unescape(escape);
console.log(result);
```
## fs.unlink(删除文件)
```
const fs = require('fs')

fs.stat('./hehe.js', (err, status) =>{
    if(err){
        console.log('文件不存在')
    }else{
        fs.unlink('./hehe.js', (err) =>{
            console.log(err)
        })
    }
})
```
## fs.writeFile('./hehe.js', 'xxx', ()=>{})
```
const fs = require('fs')

fs.stat('./hehe.js', (err, status) =>{
    if(err){
        console.log('文件不存在')
    }else{
        fs.unlink('./hehe.js', (err) =>{
            console.log(err)
            fs.writeFile('haha.js', 'xxx', () =>{
                
            })
        })
    }
})
```

# 第三方模块
## nodemailer(可以实现发送邮件)
+ [npm 官网](https://www.npmjs.com/package/npm)
```
"use strict";
const nodemailer = require("nodemailer");

    //创建发送邮件的对象
  let transporter = nodemailer.createTransport({
    host: "smtp.qq.com", //发送方邮箱比如qq
    port: 465, //端口号
    secure: true, // true for 465, false for other ports
    auth: {
      user: '1761617270@qq.com', //  发送方的邮箱地址
      pass: 'rzkdrchnmopwdedb' // mtp 验证码
    }
  });

  //邮件信息
  let mailobj = {
    from: '"Fred Foo 👻" <1761617270@qq.com>', // sender address
    to: "1761617270@qq.com", // list of receivers
    subject: "Hello ✔", // Subject line
    text: "Hello world?", // plain text body
    html: "<b>Hello world?</b>" // html body
  }
  //发送邮件
  // send mail with defined transport object
  transporter.sendMail(mailobj);


```
# node 简易爬虫
1. 获取目标网站
2. 分析网站内容
3. 获取网站有效信息（下载或者其他操作）  
## 获取目标网页
```
const http = require('https');
const fs = require('fs');
let url = 'https://www.bilibili.com/';

let rawData = '';
http.get(url, (res) =>{
    //安全判断
    const {statusCode} = res; //状态码
    const contentType = res.headers['content-type']; //数据类型
    let err = null;
    if(statusCode !== 200){
        err = new Error('请求状态错误');
    }else if(!/^text\/html/.test(contentType)){
        //格式类型是网页文件
        err = new Error('请求类型错误');
    }

    if(err){
        console.log(err);
        res.resume(); //重置缓存
        return false;
    }

    //数据分段，只要接收数据就会触发data事件，chunk每次接收的数据片段
    res.on('data', (chunk) =>{
        console.log(chunk.toString('utf8'));
        rawData += chunk.toString('utf8');
    })
    //数据流传输完毕
    res.on('end', () =>{
        fs.writeFileSync('./DATA.html', rawData);
        console.log('数据传输完毕');
    })

    

}).on('error', (error) =>{
    console.log(error);
})
```
## 通过cheerio分析网页
+ 可以使用jq中的选择器
  - 安装
  - npm install cheerio --save

# 通过express 书写api
## 登录接口逻辑分析
+ 接收用户传递的数据
+ 处理数据
+ 返回数据
## 安装express
+ npm install express --save
  - 模块引用从当前目录依次向上寻找
```
const express = require('express');
const app = express();

// api 接口
app.get('/user/login', (req, res) =>{
    console.log('HELLOW');
    res.send('注册成功！！');
})

app.listen(2000, () =>{
    //监听3000端口开启服务器
    console.log('server statr');
});

// http://localhost:3000/user/login
```



# 服务器
+ 服务器本质就是一台电脑
+ 服务器软件（apache tomcat iis ngnix node）
+ 服务器ip 以及端口
  - 局域网 ：服务器通过网线或者无线网连接，每个电脑有一个IP
  - 外网

+ 通过ip寻找的是一台电脑 通过端口号找到的是这台电脑上运行的程序
  - ip :确定服务器主机的位置
  - 端口号： 确定服务器的某一个程序
  
# mongodb
+ mongodb 数据库名
+ mongod  命令行启动数据库指令
+ mongo   命令行操作数据库指令
+ mongoose  node 操作数据库插件
# 异步回调（promise）
+ 异步操作需要保持一定的执行顺序
+ 回调函数的嵌套
+ 回调地狱
+ 解决回调地狱（promise, asyc/awaite, 蓝鸟插件 ）
## promise对象的使用
+ 大量的异步操作，如果需要顺序执行，通过回调函数执行，回调地狱
+ 通过promise 解决回调地狱

```
const fs = require('fs')

function delFile(){
    return new Promise((resolve, reject) =>{
        //异步操作
        fs.unlink('./hehe.js', (err) =>{
            if(err){
                reject('失败了')
            }else{
                resolve('成功了')
            }

        })
    })
}
delFile()

.then((msg) =>{
    console.log("then" + msg)
})
.catch((err) =>{
    console.log(err)
}) 
```
+ 在一组链式当中只有一个catch
+ 终止链式调用（在then中throw 一个错误）
```
.then(() =>{
  throw new Error('手动终止') 
})
```
1. 封装promise 函数
```
function test(){
  return new Promise((resolve, reject) =>{
    //成功获取 resolve
    //失败获取 reject
  })
}
```
2. 根据顺序形成链式调用
```
test().then().then().catch()
```
3. 根据需求捕获错误
# mongoose 的使用
+ npm install mongoose
## 连接
```
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/1902', {useNewUrlParser:true})

var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  // we're connected!
  console.log('ok')
})

```
# 用户登录注册接口

1. 创建服务
```
const express = require('express')
const app = express()
const userRouter = require('./router/userRouter')
const db = require('./db/connect')

const bodypaser = require('body-parser')
app.use(bodypaser.urlencoded({extended : false}))
app.use(bodypaser.json())

app.use('/user',userRouter)

app.listen(3000,()=>{
    console.log('连接服务成功')
})

```
2. 连接数据库
```
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/1902', {useNewUrlParser:true})

var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  // we're connected!
  console.log('mongodb connect ok')
})
```
3. Schema对象
```

var userSchema = new mongoose.Schema({
    username : {type : String, required : true},
    password : {type : String, required : true},
    agenumber : {type : Number} ,
    sex : {type : String, default : 0}
})

var UserMessage = mongoose.model('userMessage', userSchema)

module.exports = UserMessage
```
4. 创建接口
```
const express = require('express')
const router  = express.Router()
const usermessage = require('../db/model/userModel')

router.post('/reg', (req, res)=>{
    // res.send('接口测试欧克')
    // console.log('接口测试成功')
    // console.log(req.body) 
    let {username, password, agenumber, sex} = req.body
    if(username && password){
        usermessage.find({username})
        .then((data) =>{
            if(data.length === 0){
                usermessage.insertMany({username, password, agenumber, sex})
                res.send('注册成功')
                console.log(username, password, agenumber, sex)
            }else{
                res.send('该用户名已经存在')
            }
        })
    }else{
        res.send("用户名和密码是必填项")   
    }
       
})
router.post('/login', (req, res) =>{
    let {username, password} = req.body
    console.log(username,password)
    if(!username || !password){
        res.send("账号或密码未填")
    }else{
        usermessage.find({username:username, password:password})
        .then((data) =>{
           if(data.length === 0){
               res.send("用户不存在")
           }else{
               res.send("登录成功")
           }
        })
        
    }

})


module.exports = router

```
