---
title: css奇技淫巧之currentColor
date: 2021-08-05 00:10:16
categories: [技术,web前端,css]
tags: [css]
---

##### currentColor

相信不少前端同学没听过这个，也许会疑惑，css里面有这东西？哈哈，有的，它用来表示当前文字的颜色，不管当前文字的颜色是直接设置还是继承的。下面代码说明：

```
.box{
	color:yellow;
	background:currentColor; //这里currentColor就是yellow
}
//同样的
.box{
	color:yellow;
	.inner-box{
		background:currentColor;//文字颜色继承而来，所以这里的currentColor也是yellow
	}
}
```

还有，border和box-shadow的默认值都是currentColor，也就是说，这两个属性不设置颜色值时默认采用currentColor,比如：

```
.box{
	color:yellow;
	border:1px solid;//等同于  border:1px solid yellow;
	box-shadow:0 0 0;//等同于 box-shadow:0 0 0 yellow;
}
```
