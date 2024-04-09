---
title: css奇技淫巧之background-clip
date: 2021-09-05 22:30:10
categories: [技术,web前端,css]
tags: [css]
---

这是一个有趣的属性，它让我们可以为元素的背景设置自定义图形。

我们的自定义图形可以延伸到元素的边框，内边距盒或内容盒。

看看代码实现

```
<p class="border-box">背景延伸到边框。</p>
<p class="padding-box">背景延伸到边框的内部边缘。</p>
<p class="content-box">背景仅延伸到内容盒的边缘。</p>
<p class="text">背景被裁剪为前景文本。</p>
```
css部分：
```
p {
    border: .8em darkviolet;
    border-style: dotted double;
    margin: 1em 0;
    padding: 1.4em;
    background: linear-gradient(60deg, red, yellow, red, yellow, red);
    font: 900 1.2em sans-serif;
    text-decoration: underline;
}

.border-box {
    background-clip: border-box;
}

.padding-box {
    background-clip: padding-box;
}

.content-box {
    background-clip: content-box;
}

.text {
    background-clip: text;
    -webkit-background-clip: text;
    color: rgba(0, 0, 0, .2);
}
```

最终得到的效果

![Xnhtk.png](https://s1.328888.xyz/2022/04/09/Xnhtk.png)