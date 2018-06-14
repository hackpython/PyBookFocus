# 锋利的jQuery

为了让自己在写前端界面逻辑时更加游刃有余，决定有必要系统的了解一下jQuery

每学多点知识，就少写点代码

## 认识jQuery

### 1
JavaScript使网页和用户之间实现一种实时的、动态的和交互的关系，使网页可以包含更多活跃元素，但JavaScript自身存在3个弊端。

+ 复杂的文档对象模型（DOM）
+ 不一致的浏览器实现和便捷的开发
+ 调试工具的缺乏

为了简化JavaScript的开发，诞生了一些JavaScript程序库，这些程序库中封装了很多预定义的对象和实用函数，能帮助我们轻松的简历有高难度交互的web2.0特性的富客户端页面，并兼容各大浏览器。

jQuery是一个轻量级的库，拥有强大的选择器，出色的DOM操作，可靠的事件处理，完善的兼容性和**链式操作**

jQuery语法简洁、跨平台，极大的监护了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发AJax的操作。

### 2

jQuery优势

+ 轻量级，压缩后，30kb作用
+ 强大的选择器
+ 出色的DOM操作封装，jQuery封装了大量常用的DOM操作
+ 可靠的事件处理机制，在预留退路、循序渐进以及非入侵式（ubotrusive）方面也做的不错
+ 完善的Ajax
+ 不污染顶级变量。jQuery只建立一个名为jQuery的对象，其所有的函数方法都在这个对象之下，器别名$也可以随时交出控制权，不会污染其他对象，该特性使得jQuery可以与其他JavaScript库共存
+ 出色的浏览器兼容
+ 链式操作，jQuery最具特色的链式操作方式---即对发生在同一个jQuery对象上的一组动作，可以实际连写而无需重复获取对象，这使得jQuery代码无比优雅
+ 隐式迭代，当jQuery找到带有`.myClass`类的全部元素，然后**隐藏他们**，无需循环遍历每个返回的元素。相反，jQuery力的方法都被设计成自动操作对象集合，而不是单独的对象，这使得大量的循环结构变得不再必要
+ 行为层与结构层的分离。可以使用jQuery选择器选中的元素，然后直接给元素添加事件。这种行为层与结构层完全分离的思想，可以使jQuery开发人员和HTML或其他页面开发人员各司其职。
+ 丰富的插件支持
+ 开源、文档完善

### 3
去 [jQuery官方网站](http://jquery.com) 下载最新的jQuery库文件

一段简单的jQuery代码

```js
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Learn jQuery</title>
	<script type="text/javascript" src="jquery.min.js"></script>
	<script type="text/javascript">
		$(document).ready(function(){
			alert("Hello World!");
		});
		
	</script>
</head>
<body>
	
</body>
</html>
```

核心是这段

```js
$(document).ready(function(){
		alert("Hello World!");
});
```

作用类似于传统JavaScript的window.onload方法，不过与window.onload还是有些区别的


不同之处：

window.onload

执行时机：必须等待网页中所有的内容加载完毕后（包括图片）才能执行

不能同时编写多个

```js
    window.onload = function(){
			alert('test1')
		}

		window.onload = function(){
			alert('test2')
		}
```

只会输出 “test2”


$(document).ready()

执行时机：网页中所有DOM结构绘制完毕后就执行，可能DOM元素关联的东西并没有加载完

可以同时编写多个

```js
		$(document).ready(function(){
			alert("test1");
		});

		$(document).ready(function(){
			alert("test2");
		});
```

会正常输出 test1、test2

可以简写成：

```js
		$(function(){
			alert('test3');
		});
```

### 4

jQuery链式操作风格

```js
<div>
		<ul>
			<li class="level1" ><a href="#none">1</a>
				<ul class="level2">
					<li><a href="#none"></a></li>
					<li><a href="#none"></a></li>
					<li><a href="#none"></a></li>
					<li><a href="#none"></a></li>
					<li><a href="#none"></a></li>
				</ul>
			</li>
			<li class="level1"><a href="">2</a>
				<ul class="level2">
					<li><a href="">21</a></li>
					<li><a href="">22</a></li>
					<li><a href="">23</a></li>
					<li><a href="">24</a></li>
					<li><a href="">25</a></li>
				</ul>
			</li>
			<li class="level1"><a href="">3</a>
				<ul class="level2">
					<li><a href="">31</a></li>
					<li><a href="">32</a></li>
					<li><a href="">33</a></li>
					<li><a href="">34</a></li>
					<li><a href="">35</a></li>
				</ul>
			</li>
		</ul>
	</div>
	
	------js------------
	
	<script type="text/javascript">
		$(".level1 > a").click(function(){
			$(this).addClass("current").next().show().parent().siblings().children("a").removeClass("current").next().hide();
			return false;
		})
	</script>
```

上面jQuery代码的作用是：

当鼠标点击class还有level1下的a元素时，给其添加一个名为current的class，然后将紧邻其后的元素显示出来（next()方法和show()方法，同时将它的父辈的同辈元素内部的子元素<a>都去掉名为current的class，并将紧邻它们后面的元素都隐藏起来。构成了一个导航栏的效果。

这就是jQuery强大的链式操作，一行代码完成一个导航栏的功能

为了让jQuery可读性更强，有如下几个建议

+ 对于同一个对象不超过3个操作的，可以直接写成一行

```js
$("li").show().ublind("click");
```

+ 对于同一个对象有较多操作，建议每行写一个操作

```js
$(this).removeClass("mouseout")
        .addClass("mouseover")
        .stop()
        .click(function(){
            //do something
        });
```

+ 对于多个对象的少量操作，可以每个对象写一行，如果涉及子元素，可以考虑适当的缩进

```js
$(this).addClass("highligth")
        .children("li").show().end()
```

### 5
jQuery以其强大的选择器著称

```js
$("#table>tbody>tr:has(td:has(:checkbox:enabled))").css("background"."red");
```

在一个id为table的表格的tbody元素中，如果每行的一列中的checkbox没有被禁用，贼









