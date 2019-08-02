# Nodejs net聊天室

> 分为服务端与客户端

### 服务端

```javascript
const net = require('net');

const server = net.createServer((socket)=>{
	socket.on('data',(chunk)=>{
		//监听客户端发送过来的数据
		//监听里的socket代表发送数据的客户端
        socket.write('发送给客户端');
	});
	
	socket.on('end'=>{
		//监听客户端退出
	});
});

server.listen('端口','ip')
```

### 客户端

> 分两步1.new socket 2.连接的端口与ip

```javascript
const net = require('net');
//可读流与可写流
const cout = process.stdout;
const cin = process.stdin;

//1.var client = new net.Socket();

cout.write('请输入名称: ');

//监听可读流
cin.on('data',(chunk)=>{
	client.write('发送给服务端的数据');
});

client.on('data',(chunk)=>{
	//监听服务端发送过来的数据
});

//可测试是否连接成功
client.on('connect',function(){
	//console.log('连上了')
});

//2.连接的端口与ip
client.connect('9090','127.0.0.1');
```

