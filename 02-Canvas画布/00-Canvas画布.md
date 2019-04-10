# 一、什么是 Canvas？
  - HTML5 的 canvas 元素使用 JavaScript 在网页上绘制图像
  - 画布是一个矩形区域，可以控制其每一像素
  - canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法

# 二、canvas基本使用
- 添加canvas元素(创建画布)
	```
	// 在页面中添加一个canvas标签(默认画布是300*150)
	<canvas id="view1" width="300" height="300">
		  <i>不支持canvas浏览器</i>
	 </canvas>
	```

- 通过JavaScript绘制图像
	```
	<script>
		// 1、获取到canvas元素
		var oView = document.getElementById("view1");
		// 2、通过getContext获取绘制环境(多种绘制路径、矩形、圆形、字符以及添加图像的方法)
		var oGC = oView.getContext('2d');
		// 3、填充矩形
		oGC.fillRect(30, 30, 100, 100);
	</script>
	```

# 三、绘制的方法
- 绘制矩形
	```
	# 1、fillRect(X, Y, W, H) --- 填充矩形(默认是黑色)
	X: 矩形左上角的X坐标
	Y: 矩形左上角的Y坐标
	W: 矩形宽度
	H: 矩形高度
	
	# 2、 strokeRect(L, T, W, H) --- 边框矩形(默认1px黑色边框)
	X: 矩形左上角的X坐标
	Y: 矩形左上角的Y坐标
	W: 矩形宽度
	H: 矩形高度
	```
	> 注: strokeRect(30, 30, 100, 100); 表从(30, 30)位置开始，绘制宽高为100的矩形；但是边框实际显示2px并不为1px，原因是边框绘制是围绕边框线往两边延伸的，即左边0.5px右边0.5px，但实际显示都只能是整数，即变为左边1px右边1px。【解决方式 strokeRect(30.5, 30.5, 100, 100)】

- 设置绘图
    ```
	1、fillStyle: 填充样式
	2、lineWidth: 线宽
	3、strokeStyle: 边线样式
	```

- 边界绘制
    ```
	1、lineJoin: 边界连接点样式
    miter(默认)/round(圆角)/bevel(斜角)
	    
	2、lineCap: 端点样式(边线的两个端点)
    butt(默认)/round(圆角)/square(高度多出为宽一半的值)
	```
	
- 绘制路径
    ```
    1、beginPath(): 开始绘制路径
    2、closePath:() 结束绘制路径(***有闭合作用***)
    3、moveTo(x, y): 移动到绘制的新目标点
    4、lineTo(x, y): 新的目标点
    5、stroke(): 画线(默认黑色)
    6、fill(): 填充(默认黑色)
    7、rect(): 矩形区域
    8、clearRect(x, y, w, h): 删除画布的矩形区域
    9、save(): 保存当前图像状态的一份拷贝
    10、restore(): 恢复上次保存的图片状态
    ```
    > 案例: 简易画板
    
    > 案例: 移动的矩形

# 四、其他曲线
- 绘制圆形
    ```
    arc(x, y, 半径, 起始弧度, 结束弧度, 旋转方向)
      - x/y: 起始位置
      - 半径: 圆形的半径大小
      - 弧度与角度的关系: 弧度 = 角度*Math.PI / 180;
      - 旋转方向: 顺时针(默认)false、逆时针true【起始位置是在3点钟方向!!!】
    ```
    ![旋转方向](http://upload-images.jianshu.io/upload_images/1801379-3cdc4328b9b18a78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    > 案例: 绘制一个钟表

- 绘制其他曲线
    ```
    1、arcTo(x1, y1, x2, y2, r) // 第一组坐标、第二组坐标、半径
    oGC.moveTo(50, 50);  // 起始点
    oGC.quadraticCurveTo(30, 220 ,250, 250); // 第二组左边、结束坐标
    oGC.stroke();

    2、quadraticCurveTo(dx, dy, x1, y1) // 贝塞尔曲线: 第一组控制点、第二组结束坐标
	oGC.moveTo(100, 200);  // 起始点
	oGC.quadraticCurveTo(100, 100, 200, 200);
	oGC.stroke();
    
    3、bezierCurveTo(dx1, dy1, dx2, dy2, x1, y1) // 贝塞尔曲线: 第一组控制点、第二组控制点、第三组结束坐标
	oGC.moveTo(50, 50);  // 起始点
	oGC.bezierCurveTo(30, 140, 270, 180, 250, 250);
	oGC.stroke();
    ```
    > 动态绘制贝塞尔曲线的在线演示: http://myst729.github.io/bezier-curve/
    ![贝塞尔曲线1](http://upload-images.jianshu.io/upload_images/1801379-9bdb2ce92dc203db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    ![贝塞尔曲线2](http://upload-images.jianshu.io/upload_images/1801379-a877a91ce0b82587.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 五、变换
- translate(x, y)
    ```
    偏移: 从起始点为基准，移动到当前坐标位置
      oGC.translate(100, 100);
     // 起始点原本是(0,0)，因为偏移，所以变为(100, 100)
      oGC.fillRect(0, 0, 100, 100);
    ```

- rotate(angle)
    ```
    旋转: 参数是弧度
      oGC.rotate(10*Math.PI/180);
      oGC.fillRect(100,100,100,50);
    ```

- scale(x, y)
	```
	缩放: 默认不缩放即是1
	oGC.scale(0.7, 0.7);
	oGC.fillRect(100, 100, 100, 100);
	```
	> 案例: 旋转并缩放的方块

# 六、绘制图片
- drawImage(oImg, x, y, w , h): 插入图片
	```
	// oImg: 插入的图片
	// x/y: 坐标位置
	// w/h: 图片宽高

	// 创建图片对象
	var oImg = new Image();
	// 获取图片
	oImg.src = 'qq.png';
	// 绘制图片
	oGC.drawImage(oImg, 0, 0, 100, 100);

	图片预加载: 在onload中调用方法(先加载完图片，后通过canvas绘制)
	oImg.onload = function(){ //  图片加载完成 }
	``` 
	> 案例: 图片旋转效果

- createPattern(oImg, repeat): 设置背景
	```
	平铺方式: repeat/repeat-x/repeat-y/no-repeat

	// 创建图片对象
	var oImg = new Image();
	// 获取图片
	oImg.src = 'qq.png';
	// 设置背景
	var bg = oGC.createPattern(oImg, 'repeat-x');
	// 以背景形式填充
	oGC.fillStyle = bg;
	// 如果需要移动，通过translate操作
	oGC.translate(100, 100);
	// 绘制矩形
	oGC.fillRect(0, 0, 100, 100);
	```
 
# 七、渐变
- createLinearGradient(x1, y1, x2, y2): 线性渐变
	```
	// x1/y1: 起始点坐标
	// x2/y2: 结束点坐标
	// addColorStop(位置， 颜色): 添加渐变点

	// 创建线性渐变
	var obj = oGC.createLinearGradient(60, 60, 170, 170);
	// 添加渐变点(可以添加多个)
	obj.addColorStop(0, 'purple');
	obj.addColorStop(0.5, 'yellow');
	obj.addColorStop(1, 'red');		
	// 设置填充
	oGC.fillStyle = obj;
	// 绘制矩形
	oGC.fillRect(30, 30, 200, 200);
	```

- createRadialGradient(x1, y1, r1, x2, y2, r2): 放射性渐变
	```
	// x1/y1/r1: 第一个圆的坐标和半径
	// x2/y2/r2: 第二个圆的坐标和半径

	// 放射性渐变
	var obj = oGC.createRadialGradient(150, 150, 150, 150, 150, 60);
	// 添加渐变点(可以添加多个)
	obj.addColorStop(0, 'white');
	obj.addColorStop(0.5, 'yellow');
	obj.addColorStop(1, 'red');		
	// 设置填充
	oGC.fillStyle = obj;
	// 绘制矩形
	oGC.fillRect(0, 0, 300, 300);
	```

# 八、绘制文本
- strokeText(text，x, y): 文字边框
	```
	oGC.strokeText('你是谁啊?', 2, 2);
	```

- fillText(text, x, y): 文字填充
	```
	oGC.fillText('你是谁啊?', 0, 0);
	```

- font: 文字属性
	```
	// 文字大小和文字类型(必须得写，另外样式很少)
	oGC.font = '30px impact';
	```

- textAlign(end/right/center): 水平对齐方式
	```
	oGC.textAlign = 'left';
	```

- textBaseline(top/middle/bottom): 垂直对齐方式
	```
	oGC.textBaseline = 'top';
	```

- measureText(): 文本宽度(只有宽度)
	```
	var width = oGC.measureText('你是谁啊?').width
	```
	> 案例: 文本居中设置

- shadowOffsetX: x轴偏移
	```
	  oGC.shadowOffsetX = 5;
	  oGC.shadowColor = 'red';
	```
	> 文字阴影属性text-shadow: x y blur color
	> x		横向偏移
	> y		纵向偏移
	> blur		模糊距离
	> color		阴影颜色
	> 例如: text-shadow: 3px 3px 5px red;

- shadowOffsetY: y轴偏移
	```
	  oGC.shadowOffsetY = 5;
	  oGC.shadowColor = 'red';
	```

- shadowBlur: 高斯模糊值
	```
	oGC.shadowBlur = 3;
	```
	> 图像处理软件会提供"模糊"（blur）滤镜，使图片产生模糊的效果，"模糊"的算法有很多种，其中有一种叫做[高斯模糊](http://en.wikipedia.org/wiki/Gaussian_blur)（Gaussian Blur）。高斯模糊的原理中，它是根据[高斯模糊](http://en.wikipedia.org/wiki/Gaussian_blur)调节像素色值，它是有选择地模糊图像。

- shadowColor: 阴影颜色
	```
	  oGC.shadowColor = 'red';
	```

# 九、像素操作
- getImageData(x, y, w, g): 获取图片数据
	```
	  var oImg = oGC.getImageData(0, 0, 100, 100);
	```

- putImageData(获取图像, x, y): 设置新的图片数据
	```
	oGC.putImageData(oImg, 100, 100);
	```

- 像素属性
	```
	  - width: 一行的像素个数
	  - height: 一列的像素个数
	  - data: 一个数组，包含每个像素的rgba四个值，注意每个值都在0~255之间的整数
	```

- createImageData(w,h): 生成新的像素矩阵【初始值全透明的黑色，即(0,0,0,0)】
	```
	var oImg = oGC.createImageData(100, 100);
	```
	> 案例: 像素点方式显示文字

# 十、事件操作
- isPointlnPath: 是否在点击范围内
	```
	oView.onmousedown = function(ev){
		ev = ev || window.event;
		// 获取鼠标相对于画布的位置
		var dowx = ev.offsetX;
		var dowy = ev.offsetY;
						
		// 判断是否在范围之内
		if( oGC.isPointInPath(dowx, dowy) ){
			alert('我被点击了');
		}
	 }
	```

# 十一、将画布内容保存为图片
```
// 参数: canvas元素对象
function convertCanvasToImage(canvas) {
  var image = new Image();
  image.src = canvas.toDataURL("image/png");		
			
  // 返回一个img对象		
  return image;
}
```

# 十一、图表库框架
Echarts，JS图标库框架，底层依赖与canvas类库(http://echarts.baidu.com/)