<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>love</title>
<link href="favicon.ico" rel="shortcut icon" />
<style>
body{
  overflow: hidden;
  margin: 0;
}
h1{
  position: fixed;
  top: 50%;
  left: 0;
  width: 100%;
  text-align: center;
  transform:translateY(-50%);
  font-family: 'Love Ya Like A Sister', cursive;
  font-size: 40px;
  color: #c70012;
  padding: 0 20px;
}
h1 span{
	font-size:20px;
}
</style>

</head>
<body>
<h1 id="h1"></h1>
<canvas></canvas> <!--canvas 画布-->

<script>
var canvas = document.querySelector("canvas"),
  ctx = canvas.getContext("2d");

var ww,wh;

function onResize(){
  ww = canvas.width = window.innerWidth;
  wh = canvas.height = window.innerHeight;
}

ctx.strokeStyle = "red";
ctx.shadowBlur = 25;
ctx.shadowColor = "hsla(0, 100%, 60%,0.5)";

var precision = 100;
var hearts = [];
var mouseMoved = false;
function onMove(e){
  mouseMoved = true;
  if(e.type === "touchmove"){
    hearts.push(new Heart(e.touches[0].clientX, e.touches[0].clientY));
    hearts.push(new Heart(e.touches[0].clientX, e.touches[0].clientY));
  }
  else{
    hearts.push(new Heart(e.clientX, e.clientY));
    hearts.push(new Heart(e.clientX, e.clientY));
  }
}

var Heart = function(x,y){
  this.x = x || Math.random()*ww;
  this.y = y || Math.random()*wh;
  this.size = Math.random()*2 + 1;
  this.shadowBlur = Math.random() * 10;
  this.speedX = (Math.random()+0.2-0.6) * 8;
  this.speedY = (Math.random()+0.2-0.6) * 8;
  this.speedSize = Math.random()*0.05 + 0.01;
  this.opacity = 1;
  this.vertices = [];
  for (var i = 0; i < precision; i++) {
    var step = (i / precision - 0.5) * (Math.PI * 2);
    var vector = {
      x : (15 * Math.pow(Math.sin(step), 3)),
      y : -(13 * Math.cos(step) - 5 * Math.cos(2 * step) - 2 * Math.cos(3 * step) - Math.cos(4 * step)) 
    }
    this.vertices.push(vector);
  }
}

Heart.prototype.draw = function(){
  this.size -= this.speedSize;
  this.x += this.speedX;
  this.y += this.speedY;
  ctx.save();
  ctx.translate(-1000,this.y);
  ctx.scale(this.size, this.size);
  ctx.beginPath();
  for (var i = 0; i < precision; i++) {
    var vector = this.vertices[i];
    ctx.lineTo(vector.x, vector.y);
  }
  ctx.globalAlpha = this.size;
  ctx.shadowBlur = Math.round((3 - this.size) * 10);
  ctx.shadowColor = "hsla(0, 100%, 60%,0.5)";
  ctx.shadowOffsetX = this.x + 1000;
  ctx.globalCompositeOperation = "screen"
  ctx.closePath();
  ctx.fill()
  ctx.restore();
};


function render(a){
  requestAnimationFrame(render);

  hearts.push(new Heart())
  ctx.clearRect(0,0,ww,wh);
  for (var i = 0; i < hearts.length; i++) {
    hearts[i].draw();
    if(hearts[i].size <= 0){
      hearts.splice(i,1);
      i--;
    }
  }
}


onResize();
window.addEventListener("mousemove", onMove);
window.addEventListener("touchmove", onMove);
window.addEventListener("resize", onResize);
requestAnimationFrame(render);

window.onload=function starttime(){
		time(h1,'2017,11,30');	 // 在一起的时间
		ptimer = setTimeout(starttime,1000); // 添加计时器
}

	function time(obj,futimg){
		var nowtime = new Date().getTime(); // 现在时间转换为时间戳
		var futruetime =  new Date(futimg).getTime(); // 未来时间转换为时间戳
		var msec = nowtime-futruetime; // 毫秒 未来时间-现在时间
		var time = (msec/1000);  // 毫秒/1000
		var day = parseInt(time/86400); // 天  24*60*60*1000 
		var hour = parseInt(time/3600)-24*day;    // 小时 60*60 总小时数-过去的小时数=现在的小时数 
		var minute = parseInt(time%3600/60); // 分 -(day*24) 以60秒为一整份 取余 剩下秒数 秒数/60 就是分钟数
		var second = parseInt(time%60);  // 以60秒为一整份 取余 剩下秒数
//				console.log(hour+":"+minute+":"+second)
//				alert(hour)
		obj.innerHTML="卷卷<br>我们认识有：<br>"+day+"天"+hour+"小时"+minute+"分"+second+"秒"+"了<br><span>一时间不知道从哪说起。<br>虽然你不想找男朋友.<br>但是不管怎么样我会这样和你聊下去.<br>这个计时器是我自己做的放在网上的，希望它永远不会停止.</span>"

		return true;
	}
</script>

</body>
</html>
