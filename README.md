# HTML5Study
HTML5学习笔记

Web Storage是HTML5引入的一个非常重要的功能，可以在客户端本地存储数据，类似HTML4的cookie，但可实现功能要比cookie强大的多，Web Storage官方建议为每个网站5MB。
**cookie缺点**
* 每个域名下cookie的大小限制在4KB。
* cookie会包含在每个http请求中，这样会导致发送重复的数据。
* cookie在网络传输过程中没有加密，存在安全隐患。

Web Storage又分为两种：
* sessionStorage
* localStorage

**SessionStorage：**与session类似，Session Storage保存的数据生存期限与Session期限相同，用户Session结束时，Session Storage保存的数据也就消失了。
**LocalStorage：** Local Storage保存的数据一直在本地，除非用户或程序显式地清楚，否则这些数据会一致存在。

**常用方法：**
二者方法一样，但key和value都必须为字符串，换言之，web Storage的API只能操作字符串。
* 保存数据：localStorage.setItem(key,value);
* 读取数据：localStorage.getItem(key);
* 删除单个数据：localStorage.removeItem(key);
* 删除所有数据：localStorage.clear();
* 得到某个索引的key：localStorage.key(index);

下面是写的三个小例子
1，简单读取数据
> 
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Web Storager</title>
</head>
<body>
	<h1>Web Storager 事例</h1>
	<p id="msg"></p>
	<input type="text" id="input" name="">
	<input type="button" value="保存数据" onclick="saveStorage('input')" name="">
	<input type="button" value="读取数据" onclick="loadStorage('msg')" name="">
	<script type="text/javascript" src="js/script.js"></script>
</body>
</html>
```
```
// script.js
// localStorage事例
function saveStorage(id){
	var target = document.getElementById(id).value;
	var str = target;
	localStorage.setItem("message",str);
	// localStorage.message = str;
}
function loadStorage(id){
	var target = document.getElementById(id);
	var msg = localStorage.getItem("message");
	// var msg = localStorage.message;
	target.innerHTML = msg;
}
```
效果图
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2015490-ded421dcb88f4d0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

　
2,简单留言板
> 
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>简单的留言板</title>
</head>
<body>
	<h1>简单的web留言板</h1>
	<textarea id="memo" cols="60" rows="10"></textarea><br>
	<input type="button" value="追加" onclick="saveStorage('memo');">
	<input type="button" value="初始化" onclick="clearStorage('msg');">
	<hr>
	<p id="msg"></p>
	<script type="text/javascript" src="js/script.js"></script>
</body>
</html>
```
```
function saveStorage(id){
	var data = document.getElementById(id).value;
	var time = new Date().getTime();
	localStorage.setItem(time,data);
	alert("数据已经保存!");
	loadStorage('msg');
}
function loadStorage(id){
	var result = '<table border = "1">';
	for(var i = 0;i < localStorage.length;i++){
		var key = localStorage.key(i);
		var value = localStorage.getItem(key);
		var date = new Date();
		date.setTime(key);
		var datestr = date.toGMTString();
		result += '<tr><td>' + value + '</td><td>' + datestr + '</td></tr>';
	}
	result += '</table>';
	var target = document.getElementById(id);
	target.innerHTML = result;
}
function clearStorage(){
	localStorage.clear();
	alert("全部数据被清除!");
	loadStorage('msg');
}
```
效果图
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2015490-cf2e0185f2f2edc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

　
3,简易数据库
> 
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>简易数据库示例</title>
</head>
<body>
	<h1>使用web Storage来做简易的数据库示例</h1>
	<table>
		<tr>
			<td>姓名：</td>
			<td><input type="text" id="name"></td>
		</tr>
		<tr>
			<td>EMAIL：</td>
			<td><input type="text" id="email"></td>
		</tr>
		<tr>
			<td>电话号码：</td>
			<td><input type="text" id="tel"></td>
		</tr>
		<tr>
			<td>备注：</td>
			<td><input type="text" id="memo"></td>
		</tr>
		<tr>
			<td></td>
			<td>
				<input type="button" value="保存" onclick="saveStorage();">
			</td>
		</tr>
	</table>
	<hr>
	<p>
		检索：<input type="text" id="find">
		<input type="button" value="检索" onclick="findStorage('msg')" name="">
	</p>
	<p id="msg"></p>
	<script type="text/javascript" src="js/script.js"></script>
</body>
</html>
```
```
function saveStorage(){
	var data = new Object;
	data.name = document.getElementById('name').value;
	data.email = document.getElementById('email').value;
	data.tel = document.getElementById('tel').value;
	data.memo = document.getElementById('memo').value;
	var str = JSON.stringify(data);
	localStorage.setItem(data.name,str);
	alert("数据已经保存!");
}
function findStorage(id){
	var find = document.getElementById('find').value;
	var str = localStorage.getItem(find);
	var data = JSON.parse(str);
	var result = "姓名：" + data.name + '<br>';
		result += "EMAIL：" + data.email + '<br>';
		result += "电话号码：" + data.tel + '<br>';
		result += "备注：" + data.memo + '<br>';
	var target = document.getElementById(id);
	target.innerHTML = result;
}
```
效果图
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2015490-a6bd4b16334486f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

　
