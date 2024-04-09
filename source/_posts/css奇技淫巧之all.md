---
title: css奇技淫巧之all
date: 2021-12-05 20:00:13
categories: [技术,web前端,css]
tags: [css]
---

##### all属性

all属性将除unicode-bidi和direction之外的所有属性重置为其初始值或继承的值。属性值有initial、inherit和unset。

下面用代码说明
 ```
 //html
<body>
	<div class='container'>123</div>
</body>

//css
body{
	color:blue;
}
.container{
	color:red;
	all:inherit;
}
 ```

 得到的结果将会是

 ![XOmiF.png](https://s1.328888.xyz/2022/04/09/XOmiF.png)

 如果把all设置为initial，将会得到一个初始值

![XO3Zy.png](https://s1.328888.xyz/2022/04/09/XO3Zy.png)

如果all:unset,则表示，如果可继承，则将应用于元素或元素父元素的所有属性更改为其父值，否则将其更改为初始值。