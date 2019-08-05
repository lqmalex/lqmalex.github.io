# NodejsNet主

> net模块是基于tcp协议的服务端和客户端
>
> 使用net制作群聊天分为两步:服务端，客户端

### 服务端

```javascript
//引入模块
const net = require('net');
const clients = [];

//开启服务
const server = net.createServer((socket) => {
    console.log('客户端以连接');
    //将每个用户的socket添加入数组当中
    clients.pudh(socket);
    
    //监听客户端发送过来的数据
    socket.on('data',(chunk) => {
        console.log(chunk.toString());
        
        //调用函数
        Data(chunk.msg,chunk.nick, socket);
    });
    
    // 当开始监听的时候就会调用这个回掉函数
    server.on("listening", function() {
        console.log("start listening...");  
    });
 
    // 监听发生错误的时候调用
    server.on("error", function() {
        console.log("listen error");
    });
    
    //客户端退出时触发
    socket.on('end',() => {
        console.log('客户端退出');
    });
});

/**
 * 发送信息
 */
    function Data(msg,name,socket){
        //循环数组且判断 触发data事件的socket是那个客户端
        for(var i = 0;i < clients.length;i++){
            if (clients[i] != socket) {
                clients[i].write(`${name}:${msg}`);
            } else {
                clients[i].write(`你说:${msg}`);
            }
        }
    }   

//监听端口号
server.listen(8080);
```

> 每个客户端都有自己的socket
>
> on('data')触发的对象即使某个客户端的socket
>
> 相应on('end')也是如此

### 客户端

> 客户端连接服务端也需两步
>
> 1.new net.Socket();
>
> 2.连接服务端对应的端口号

```javascript
//引入模块
const net = require('net');
//可读流与可写流
//process进程stdout为可写流
//stdin为可读流
const cout = process.stdout;
const cin = process.stdin;

//第一步
var client = new net.Socket();

//可写流将其数据打印
cout.write(`请输入名称: `);

//可读流监听
cin.on('data',(chunk) => {
    //将起监听到的数据使用正则匹配掉\r\n 并赋值给msg
    msg = chunk.toString().replace(/[\r\n]/ig,"");
    
    //发送数据给服务段 格式为json格式
    client.write(JSON.stringify({
        msg : msg,
        nick : nick
    }));
});

//监听服务端发送过来的数据 
//注意不能将其监听事件发进可读流监听 会出现重复现象
client.on('data',(chunk) => {
   //处理数据
   chunk = chunk.toString().replace(/[\r\n]/ig,""); 
   console.log(chunk);
});

//连接服务端成功触发
client.on('connect',() => {
    console.log('连上了'); 
});

//第二步设置连接的端口与ip
//ip默认位127.0.0.1
client.connect({
   port:8080 
});
```

> 注意: process.stdin为可读流
>
> C语言stdin为输入流
>
> process.stdout为可写流
>
> C语言stdout为输出流

* 服务端

![](/home/lqm/wu-kan.github.io/public/image/server.png)

* 客户端

![](/home/lqm/wu-kan.github.io/public/image/clent2.png)

