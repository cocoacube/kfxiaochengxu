---
uuid: f216eca0-e5da-11e8-8bcd-fddcf839c3de
title: 关于slot插槽
date: 2018-11-12 01:55:16
tags:
---

### 关于Slot插槽



笔者过去做的前端开发主要是iOS app的开发，对于slot插槽是很陌生的。但使用中发现在vuejs开发时，slot插槽是一个非常有魔力的东西。



w3c中是这样定义slot的：

------

Definitions

These are the new definitions. For all other definitions, consult current [spec](http://w3c.github.io/webcomponents/spec/shadow/).

- **slot** -- a defined location in a shadow tree. Represented by the `slot` element.
- **slot name** -- the name of a **slot**.
- **default slot** -- a **slot** for assigning nodes without a **slot name**.

------

意思是说，slot在语法树中定义了一个影位，这个影位用slot标签展示。

初学HTML的朋友可能不太常见到slot标签，大多数看到的是div, h1,  img等最常用的标签，但事实上在使用div标签的时候，就已经运用了类似slot功能。因为一个div标签在定义的时候就在内部增加了slot插槽，这使得我们可以在div内部放其它的标签。



举例：

我们定义一个自己的component（组件）

```Vue
<template id="cool-component">
    <p> Here is all my cool stuff</p>
</template>
```

上面我们使用template标签定义了一个最简单的component，名字叫cool-component。

这个component只有一个功能，就是展示我们想要展示的话。初学web编程的朋友可能会问：我们为什么要如此复杂的为了一句话来写这么长的代码？这样的疑问提出的完全正确！因为为了一句话写一个component就是画蛇添足。不过这里只是举例子，因为component使用的场景，应该是更复杂的地方。比如在为大的企业开发网站时，由于多个页面会展示相同的企业信息或别的可以复用的内容，如果每次去写，就太麻烦了。这时候我们会定义一个component，来提高开发效率。所以用component的场景是component内容需要多次被复用，并且可以封装的情况下，我们去定义它。团队协作中，使用component也可以大大提升开发的效率。



当我们在代码中引用cool-component标签时：

```Html
<div>
	<cool-component/>
</div>
```

对应翻译到网页上的代码是：

```Html
<div>
	<p> Here is all my cool stuff</p>
</div>
```



但是只显示cool-component标签内已经定义好的static文字，似乎有点单调。该怎么修改呢？

我们对cool-component做一点点调整，这里我们会用到slot标签。

```VUE
<template id="cool-component">
	<slot></slot>
</template>
```



可以看到我们将原有的p标签替换成了slot标签。那么有什么用呢？

我们来看看现在如何使用新构造的cool-component:



在cool-component中添加一个按钮：

```Vue
<div>
    <cool-component>
        <button>Super cool button</button>
    </cool-component>
</div>
```



在cool-component中加一段文字：

```Vue
<div>
    <cool-component>
        <p>You are awesome!</p>
    </cool-component>
</div>
```



你还可以在cool-component中增加各种你想要的内容。

所以，slot为自定义component提供了功能的拓展，使得component的使用可以更加变化多端，功能强大。