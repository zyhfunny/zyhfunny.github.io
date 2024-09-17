# 前言
关于【生命游戏】之前小编写发过一篇Java版的，这里就不再对其介绍了，不了解的读者可以点下方链接前往了解：
[https://blog.csdn.net/weixin_44155115/article/details/103884572](https://blog.csdn.net/weixin_44155115/article/details/103884572)

这一次小编做了一个web版的，并分享在我的码云上，感觉用js做确实是比Java的swing做起来方便很多，这也算是小编初学前端第一个练习吧。

# 效果图
先看看效果图吧！
有兴趣的朋友也可以点击下方链接试玩：
[http://nonoas.gitee.io/webproj/LifeGame/](http://nonoas.gitee.io/webproj/LifeGame/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200326223631377.gif)
# 源码
这里小编把源码附上，送给和我一样初学前端的朋友，也希望路过的大佬不吝赐教，哈哈。
源码也可以通过下方【码云】链接获取：
[https://gitee.com/nonoas/webProj/tree/master/LifeGame](https://gitee.com/nonoas/webProj/tree/master/LifeGame)

## HTML

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>生命游戏网页版</title>
		<link type="text/css" rel="stylesheet" href="css/index.css" />
		<link type="text/css" rel="stylesheet" href="css/style.css" />
		<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
		<script src="js/index.js"></script>
	</head>

	<body>
		<div class="center-content-box">
			<h1>生命游戏</h1>
		</div>
		<div id="table-module" class="center-content-box">

			<!-- 游戏简介 -->
			<div id="game-intro">
				<h2>游戏简介</h2>
				<p>生命游戏(game of life)为1970年由英国数学家J. H. Conway所提出
					某细胞的邻居包括上、下、左、右、左上、左下、右上与右下相邻之细胞 游戏规则如下：</p>
				<p><b>孤单死亡：</b>如果细胞的邻居小于等于1个，则该细胞在下一次状态将死亡；</p>
				<p><b>拥挤死亡：</b>如果细胞的邻居在4个及以上，则该细胞在下一次状态将死亡；</p>
				<p><b>稳定：</b>如果细胞的邻居为2个或3个，则下一次状态为稳定存活；</p>
				<p><b>复活：</b>如果某位置原无细胞存活，而该位置的邻居为2个或3个，则该位置将复活一个细胞。</p>
			</div>

			<div id="table-box">
				<table id="cell-table" border="1">
				</table>
			</div>

			<!-- 操作界面 -->
			<div id="game-operator">
				<div class="text-module">
					<p id="cur-period"><span class="Label">当前周期：</span><span id="period"></span></p>
					<p>
						<span class="Label">行列数量：</span>
						<input id="cell-count" type="text" placeholder="整数(0<n<=100)" />
					</p>
					<p><span class="Label">演化速度：</span><input id="speed" type="text" /></p>
					<p title="设置随机分布时的密度"><span class="Label">分布密度：</span><input id="thickness" type="text" /></p>
					<p class="comment">提示：“行列数量” 和 “分布密度” 在点击 “重新布置” 后生效。</p>
				</div>

				<div class="btn-module">
					<div class="btn-box">
						<p><input id="resetCell" type="button" value="重新布置" /></p>
						<p><input id="btn-randomSet" type="button" value="随机分布" /></p>
						<p><input id="next-term" type="button" value="下一周期" /></p>
						<p><input id="btn-start" type="button" value="开始演化" /></p>
					</div>
				</div>

			</div>
		</div>
		<div id="copy-right">
			<nav class="center-content-box">
				<a href="">作者：Nonoas</a>
				<hr>
				<a href="https://blog.csdn.net/weixin_44155115/article/details/105129831">CSDN</a>
				<hr>
				<a href="https://gitee.com/nonoas/webProj/tree/master/LifeGame" target="_blank">Gitee</a>
			</nav>
		</div>
	</body>

</html>

```

## CSS

```css
@charset "utf-8";

* {
	margin: 0;
	box-sizing: border-box;
	color: white;
	font-family:"Microsoft YaHei UI";
	/* border: 1px solid #aaa; */
}

html {
	height: 100%;
	width: 100%;
}

body {
	width: 100%;
	height: 100%;
	padding: 2%;
	background: url(../img/home-bg.png);
	overflow: hidden;
}

h1{
	filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.3));
	-webkit-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.3));
	-moz-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.3));
}

#table-module {
	height: 90%;
}

#table-box {
	width: 30rem;
	height: 30rem;
	padding: 1.5%;
	background-color: #23b8af;
	filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	-webkit-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	-moz-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	border-radius: 1rem;
}

#table-box td:hover {
	animation: tdHover 1s;
	background-color: rgba(255, 255, 255, 0.7);
}

table {
	width: 100%;
	height: 100%;
	text-align: center;
	border-collapse: collapse;
	border: 1px solid white;
}

#game-intro {
	width: 20%;
	height: 30rem;
	margin-right: 2%;
	padding: 2%;
	background-color: #23b8af;
	border-radius: 1rem;
	filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	-webkit-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	-moz-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	overflow: auto;
}

#game-intro::-webkit-scrollbar {
	display: none;
}

#game-intro h2 {
	margin-bottom: 1.5rem;
	text-align: center;
}

#game-intro p {
	font-size: 0.9rem;
	margin: 5% 0 5% 0;
}

#game-operator {
	width: 20%;
	height: 30rem;
	margin-left: 2%;
	padding: 1%;
	background-color: #23b8af;
	border-radius: 1rem;
	filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	-webkit-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	-moz-filter: drop-shadow(2px 5px 5px rgba(0, 0, 0, 0.2));
	overflow: auto;
}

#game-operator p {
	margin: 5%;
}

#game-operator::-webkit-scrollbar {
	display: none;
}

.text-module {
	height: 45%;
}

.text-module input[type="text"] {
	width: calc(100%);
	padding: 2%;
	border: 1.5px solid #fff;
	outline: none;
	background-color: rgba(0, 0, 0, 0);
}

input::-webkit-input-placeholder {
	color: #ddd;
}

input::-moz-placeholder {
	/* Mozilla Firefox 19+ */
	color: #ddd;
}

input:-moz-placeholder {
	/* Mozilla Firefox 4 to 18 */
	color: #ddd;
}

input:-ms-input-placeholder {
	/* Internet Explorer 10-11 */
	color: #ddd;
}


.text-module p {
	display: flex;
	align-items: center;
}

#cur-period {
	display: flex;
	justify-content: flex-start;
}

.text-module .Label {
	font-weight: bold;
	white-space: nowrap;
}

.text-module .comment{
	font-size: 0.7rem;
}

#cell-count {
	width: calc(100%-10px);
}

.btn-module {
	height: 50%;
	display: flex;
	align-items: flex-end;
}

.btn-box {
	width: 100%;
}

.btn-module input[type="button"] {
	width: 100%;
	padding: 0.5rem;
	font-size: 1.1rem;
	color: #19867d;
	border: none;
	border-radius: 1rem;
	background-color: #b8faff;
	transition: all 0.4s;
	outline: none;
}

.btn-module input[type="button"]:hover{
	background-color: #fff;
}

.btn-module input[type="button"]:active{
	background-color: #b8faff;
}

#copy-right a {
	margin: 0 1% 0 1%;
	font-size: 0.8rem;
}

#copy-right hr{
	height: 1rem;
	border: none;
	border-right: 1px solid #fff;
}

```
## JS

```javascript
var row = 15; //全局设置网格的数量
var speed = 200;
var period;
var thickness = 3;

var r = "rgba(255, 255, 255, 0.9)";
var n = "rgba(255, 255, 255, 0)";

// 状态数组
var lifeRule = new Array();

//细胞皿数组
var cell = new Array();

var tr; //表格行

var suspand; //演化标记，用于停止演化


//文档初始化代码块
$(function() {

	initAll()

});

// 全局初始化
function initAll() {

	period = 0;

	$("#cell-count").attr("value", row);
	$("#speed").attr("value", speed);
	$("#period").text(period);
	$("#thickness").attr("value", thickness);

	//初始化细胞皿数组
	for (var i = 0; i < row; i++) {
		cell[i] = new Array();
		for (var j = 0; j < row; j++) {
			cell[i][j] = 0;
		}
	}

	//初始化状态数组
	for (var i = 0; i < row; i++) {
		lifeRule[i] = new Array();
		for (var j = 0; j < row; j++) {
			lifeRule[i][j] = 0;
		}
	}

	// 初始化表格
	for (var i = 0; i < row; i++) {
		var tr = $("<tr>");
		for (var j = 0; j < row; j++) {
			tr.append($("<td>"));
		}
		tr.appendTo($("#cell-table"));
	}

	// 单元格点击事件
	$("td").click(function() {

		var color = $(this).css("background-color");

		var rowIndex = $(this).parent().index();
		var cellIndex = $(this).index()

		// 改变颜色和cell真值数组
		if (color == r) {
			cell[rowIndex][cellIndex] = 0;
			$(this).css("background-color", n);
		} else {
			cell[rowIndex][cellIndex] = 1;
			$(this).css("background-color", r);
		}
	});

	tr = $("#cell-table").find("tr");

	// 随机布置细胞皿
	$("#btn-randomSet").click(function() {

		for (var i = 0; i < row; i++)
			for (var j = 0; j < row; j++) {
				var flag = getRandomNum(0, 10);
				if (flag < thickness) {
					cell[i][j] = 1;
					tr.eq(i).find("td").eq(j).css("background-color", r);
				} else {
					cell[i][j] = 0;
					tr.eq(i).find("td").eq(j).css("background-color", n);
				}
			}
	});
}

$(function() {

	$("#cell-count").change(function() {
		var rows = $(this).val();
		if (rows <= 0 || rows > 100){
			alert("行列数量取值范围为：(0,100]");
			$(this).val(row);
		}
			
		
	});

	$("#speed").change(function() {
		var s = $(this).val();
		if (s < 0 || s> 5000) {
			alert("演化速度取值范围为：[0,5000]");
			$(this).val(speed);
		} else {
			speed=$(this).val();
		}
	});

	// 开始演化
	$("#btn-start").click(function() {
		var text = $(this).val();
		console.log(text);
		if (text == "开始演化") {
			$(this).val("暂停");
			suspand = setInterval(evolution, speed);
		} else {
			$(this).val("开始演化");
			clearInterval(suspand);
		}
	});

	// 下一周期
	$("#next-term").click(function() {
		evolution();
	});

	//重置
	$("#resetCell").click(function() {

		clearInterval(suspand);

		$("#btn-start").val("开始演化");

		row = $("#cell-count").val();
		speed = $("#speed").val();
		period = $("#period").text();
		thickness = $("#thickness").val();


		$("#cell-table").empty();

		initAll();
	});
})




// 获取最小值到最大值之前的整数随机数
function getRandomNum(Min, Max) {
	var Range = Max - Min;
	var Rand = Math.random();
	return (Min + Math.round(Rand * Range));
}

function evolution() {
	var tr = $("#cell-table").find("tr");
	setCellBool();
	for (var i = 0; i < lifeRule.length; i++)
		for (var j = 0; j < lifeRule[0].length; j++) {
			if (lifeRule[i][j] == 1) {
				cell[i][j] = 1;
				tr.eq(i).find("td").eq(j).css("background-color", r);
			} else {
				cell[i][j] = 0;
				tr.eq(i).find("td").eq(j).css("background-color", n);
			}
		}
	$("#period").text(++period);
}


//设置下一周期的细胞状态
function setCellBool() {
	for (var i = 0; i < lifeRule.length; i++)
		for (var j = 0; j < lifeRule[0].length; j++) {
			switch (countAround(i, j)) {
				case 0:
				case 1:
				case 4:
				case 5:
				case 6:
				case 7:
				case 8:
					lifeRule[i][j] = 0;
					break;
				case 2:
				case 3:
					lifeRule[i][j] = 1;
					break;
			}
		}
}

// 统计某单元格周围的细胞数量
function countAround(r, c) {
	var count = 0;
	//上一行
	count += countCell(r - 1, c - 1);
	count += countCell(r - 1, c);
	count += countCell(r - 1, c + 1);
	//同行
	count += countCell(r, c - 1);
	count += countCell(r, c + 1);
	// 下一行
	count += countCell(r + 1, c - 1);
	count += countCell(r + 1, c);
	count += countCell(r + 1, c + 1);

	return count;
}

//统计一个单元格内的细胞数量
function countCell(r, c) {
	if (r < 0 || r >= row || c < 0 || c >= row || cell[r][c] == 0)
		return 0;
	return 1;
}

```
