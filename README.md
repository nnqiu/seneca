seneca是nodejs的微服务框架
每个服务提供不同的功能，卖锅的，卖菜的，收钱的，发短讯的......



创建文件夹
```
npm init
npm install seneca --save
```
创建一个js文件 我这里是seneca.js
```
const seneca = require('seneca')();

seneca.add('role: math, cmd: sum', (msg, reply) => {
	reply(null, { answer: ( msg.left + msg.right ) })
});

seneca.act({
	role: 'math',
	cmd: 'sum',
	left: 1,
	right: 2,
}, (err, result) => {
	if (err) {
		return console.error(err);
	}
	console.log(result)
});
```
运行  node seneca.js
![image.png](https://upload-images.jianshu.io/upload_images/13540125-6cd4ee6a319e256f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面的代码都是什么意思呢,有add和act函数，规定了role，cmd的模式，匹配之后，执行add中的回调函数执行left+right代码块，就是**模式匹配**
```
const seneca = require('seneca')();


	向seneca中添加动作模式函数 1.pattern 2.action	
	1.pattern 用于匹配senenca实例中的json消息体 
		{
		  role: 'math',
		  cmd: 'sum',
		}
		role cmd 没什么意义，只是匹配的模式而已
	2.action 用于模式被匹配时执行的操作 （箭头函数）
	计算匹配到的函数体中两个属性left right，得到{ answer: 3 }

seneca.add('role: math, cmd: sum', (msg, reply) => {
	reply(null, { answer: ( msg.left + msg.right ) })
});

seneca.add('role:math, cmd:product', (msg, reply) => {
  reply(null, { answer: ( msg.left * msg.right )})
});

	发送消息 1.msg 2.response
	1.msg 作为纯对象提供的待匹配的入站信息，发送的消息主体
	2.respond 用于接受并处理响应信息的回调函数，就是如果消息有响应了就执行此函数
seneca.act({
	role: 'math',
	cmd: 'sum',
	left: 1,
	right: 2,
}, (err, result) => {
	if (err) {
		return console.error(err);
	}
	console.log(result)
});
```