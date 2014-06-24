微信公众帐号“玩转三里屯”--消费指南
===============

使用canvas为背景添加动画。

####Step1.HTML页面
  完成HTML页面
  
  
  [点击查看](https://github.com/cyclegtx/wave_background/tree/ef35a31908e6a735a3f4b576d80fd622375e731f)  
####Step2.添加Canvas
  新建一个画布（<canvas>)元素，并放在在所有按钮和logo的下方以免遮挡前面的元素。
  
  
  将Canvas的宽高设定成其父元素的宽高，以充满他的父元素。也可以直接使用```window.innerHeight``` ```window.innerWidth```
  使其充满整个屏幕。
  
  
  [点击查看](https://github.com/cyclegtx/wave_background/tree/2cf051efbf4dd95838b56e16f1d83feec0780a82)
####Step3.画矩形
  在画布中画一个充满半个屏幕的矩形。
  我们只需要找到矩形的四个定点的坐标，使用Canvas的绘制路径并填充这个路径。四个点分别是：
* (0, 画布高度t/2)
* (画布宽度, 画布高度t/2)
* (画布宽度 画布高度t/2)
* (0, 画布高度t/2)

  注意：坐标的（0，0）在画布的左上角。  
```javascript
//填充颜色
ctx.fillStyle = "rgba(0,222,255, 0.2)";
//开始绘制路径
ctx.beginPath();
//左上角
ctx.moveTo(0, canvas.height/2);
//右上角
ctx.lineTo(canvas.width, canvas.height/2);
//右下角
ctx.lineTo(canvas.width, canvas.height);
//左下角
ctx.lineTo(0, canvas.height);
//左上角
ctx.lineTo(0, canvas.height/2);
//闭合路径
ctx.closePath();
//填充路径
ctx.fill();

```
	[点击查看](https://github.com/cyclegtx/wave_background/tree/60be5b0e29bad3012627e27cbe86daa1d6678160)
####Step4.使矩形运动
  让矩形动起来。要做动画我们需要持续的清空画布并重新绘制新的矩形，就像电影每秒播放24张图片。我们新建一个loop函数，用来绘制每一帧的图像，并使用requestAnimFrame来告诉浏览器每一帧都要使用loop来绘制。
```javascript
//如果浏览器支持requestAnimFrame则使用requestAnimFrame否则使用setTimeout
window.requestAnimFrame = (function(){
return  window.requestAnimationFrame       ||
		window.webkitRequestAnimationFrame ||
		window.mozRequestAnimationFrame    ||
		function( callback ){
          window.setTimeout(callback, 1000 / 60);
        };
})();
function loop(){
	requestAnimFrame(loop);
}
loop();
```
  把之前绘制矩形的代码放到loop中，并在绘制矩形的代码之前清空画布中所有的图形。
```javascript
function loop(){
	//清空canvas
	ctx.clearRect(0,0,canvas.width,canvas.height);
	//绘制矩形
	ctx.fillStyle = "rgba(0,222,255, 0.2)";
	ctx.beginPath();
	ctx.moveTo(0, canvas.height/2);
	ctx.lineTo(canvas.width, canvas.height/2);
	ctx.lineTo(canvas.width, canvas.height);
	ctx.lineTo(0, canvas.height);
	ctx.lineTo(0, canvas.height/2);
	ctx.closePath();
	ctx.fill();
	requestAnimFrame(loop);
}
```
  接下来我们更改每一帧中的矩形的高度来模拟波浪的形态，波浪其实是在波峰与波谷之间做周期性运动。我们假设波峰与波谷间都是50px，那么矩形的高度的变化值应该在-50px到50px之间。为了达到周期性的效果我们采用正弦函数sin(x)，因为不管x值怎么变化sin(x)的值始终在-1与1之间。我们新建一个变量 var step =0 使其在每一帧中自增，表示每一帧角度增加一度，并用Math.sin()取他的正弦值。JS中的sin使用的弧度值，我们需要把step转换成弧度值,var angle = step*Math.PI/180; 取角度的正弦值乘以50得到了矩形高度的变化量。将变化量加在矩形的左上与右上两个顶点的y坐标上。
```javascript
//初始角度为0
var step = 0;
function loop(){
	ctx.clearRect(0,0,canvas.width,canvas.height);
	ctx.fillStyle = "rgba(0,222,255, 0.2)";
	//角度增加一度
	step++;
	//角度转换成弧度
	var angle = step*Math.PI/180;
	//矩形高度的变化量
    var deltaHeight   = Math.sin(angle) * 50;
    ctx.beginPath();
    //在矩形的左上与右上两个顶点加上高度变化量
    ctx.moveTo(0, canvas.height/2+deltaHeight);
    ctx.lineTo(canvas.width, canvas.height/2+deltaHeight);
    ctx.lineTo(canvas.width, canvas.height);
    ctx.lineTo(0, canvas.height);
    ctx.lineTo(0, canvas.height/2+deltaHeight);
    ctx.closePath();
    ctx.fill();
	requestAnimFrame(loop);
}
```
	[点击查看](https://github.com/cyclegtx/wave_background/tree/5d855717cb6b788dabbe5268e6674300f5731e80)
####Step5.使矩形左右运动不同步


	[点击查看](https://github.com/cyclegtx/wave_background/tree/1f42946f10836110e16dbfd76a65fcd88cd16ae1)
####Step6.模拟波浪


	[点击查看](https://github.com/cyclegtx/wave_background/tree/32ae5d1096f906c697458201c2273ff8abb49fbb)
####Step7.3个波浪


	[点击查看](https://github.com/cyclegtx/wave_background/tree/1f316066805f48ccc312aad35f83963c6b5fb6a3)
####Step8.完成


	[点击查看](https://github.com/cyclegtx/wave_background/tree/3206e4e0a65912b34e8a426de22fd3201ab4a80e)





