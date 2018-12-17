---
uuid: 24203300-eec8-11e8-9ab0-e176216747e5
title: 从零开始开发你第一个微信小程序教程tutorial-part2
date: 2018-11-23 10:33:20
tags:
<<<<<<< HEAD
=======
featured_image: /img/asuka05.png
>>>>>>> c07f0cc8fbbebeb210328fdc9baf99530a701dd1
---

在上一篇中，我们已经创建了一个微信小程序工程，这一篇，我们将开始开发微信小程序Asuka。

在创建工程的时候，我选择了使用TypeScript语法的工程。TypeScript相对于JavaScript来说，多了类型描述，并且TypeScript完全支持JavaScript语法，所以不必担心。

首先，大家可以在Utils目录下，找到app.json文件，并且打开它。

```javascript
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle":"black"
  }
}
```



app.json文件是整个小程序的配置文件，这里有小程序一些基本设置。其中pages项描述了小程序内包含的页面，之类我们看到包含了两个页面，一个是index，一个是logs，这两个页面我们可以在pages目录中找到。由于index页面放在了第一位，所以这个就是小程序打开时所展示的页面。

然后我们进入index.wxml页面，打开后可以看到如下代码。

```html
<!--index.wxml-->
<view class="container">
  <view class="userinfo">
    <button wx:if="{{!hasUserInfo && canIUse}}" open-type="getUserInfo" bindgetuserinfo="getUserInfo"> 获取头像昵称 </button>
    <block wx:else>
      <image bindtap="bindViewTap" class="userinfo-avatar" src="{{userInfo.avatarUrl}}" mode="cover"></image>
      <text class="userinfo-nickname">{{userInfo.nickName}}</text>
    </block>
  </view>
  <view class="usermotto">
    <text class="user-motto">{{motto}}</text>
  </view>
</view>
```



这是微信小程序创建的模版代码，现在我们就来修改它，来构建我们Asuka小程序的UI布局。

我们将代码的2-12行全部删除。结果如下：

```html
<!--index.wxml-->
<view class="container">
 
    
    
    
    
    
    
    
    
    
</view>
```



![asukaui1](/img/asukaui1.png)

根据我们的UI，Asuka可以分成两个部分，上部的留言列表，下部的内容编辑区域和发送按钮。

由于现在我们还没有数据库，所以我们先做一些mock data（mock data是用来快速实现功能而创建的假数据）。

现在打开index.ts文件（如果在创建时没有选择TypeScript工程，那可以打开index.js文件），内容如下：

```javascript
//index.js
//获取应用实例
import { IMyApp } from '../../app'

const app = getApp<IMyApp>()

Page({
  data: {
    motto: '点击 “编译” 以构建',
    userInfo: {},
    hasUserInfo: false,
    canIUse: wx.canIUse('button.open-type.getUserInfo'),
  },
  //事件处理函数
  bindViewTap() {
    wx.navigateTo({
      url: '../logs/logs'
    })
  },
  onLoad() {
    if (app.globalData.userInfo) {
      this.setData!({
        userInfo: app.globalData.userInfo,
        hasUserInfo: true,
      })
    } else if (this.data.canIUse){
      // 由于 getUserInfo 是网络请求，可能会在 Page.onLoad 之后才返回
      // 所以此处加入 callback 以防止这种情况
      app.userInfoReadyCallback = (res) => {
        this.setData!({
          userInfo: res,
          hasUserInfo: true
        })
      }
    } else {
      // 在没有 open-type=getUserInfo 版本的兼容处理
      wx.getUserInfo({
        success: res => {
          app.globalData.userInfo = res.userInfo
          this.setData!({
            userInfo: res.userInfo,
            hasUserInfo: true
          })
        }
      })
    }
  },

  getUserInfo(e: any) {
    console.log(e)
    app.globalData.userInfo = e.detail.userInfo
    this.setData!({
      userInfo: e.detail.userInfo,
      hasUserInfo: true
    })
  }
})

```

代码第8行我们可以看到一个data变量。这个变量用来制定index文件内可以调用的数据。我们先把data内的无用内容删除掉，然后我们对它进行一个修改，添加一些mock data，命名为mockMessages。结果如下：

```javascript
//index.js
//获取应用实例
import { IMyApp } from '../../app'

const app = getApp<IMyApp>()

Page({
  data: {
	mockMessages: ['Hello Asuka!', 'I am asuka user1', 'Who is it there?']
  },
  //事件处理函数
  bindViewTap() {
    wx.navigateTo({
      url: '../logs/logs'
    })
  },
  onLoad() {
    if (app.globalData.userInfo) {
      this.setData!({
        userInfo: app.globalData.userInfo,
        hasUserInfo: true,
      })
    } else if (this.data.canIUse){
      // 由于 getUserInfo 是网络请求，可能会在 Page.onLoad 之后才返回
      // 所以此处加入 callback 以防止这种情况
      app.userInfoReadyCallback = (res) => {
        this.setData!({
          userInfo: res,
          hasUserInfo: true
        })
      }
    } else {
      // 在没有 open-type=getUserInfo 版本的兼容处理
      wx.getUserInfo({
        success: res => {
          app.globalData.userInfo = res.userInfo
          this.setData!({
            userInfo: res.userInfo,
            hasUserInfo: true
          })
        }
      })
    }
  },

  getUserInfo(e: any) {
    console.log(e)
    app.globalData.userInfo = e.detail.userInfo
    this.setData!({
      userInfo: e.detail.userInfo,
      hasUserInfo: true
    })
  }
})

```

mockMessage是一个字符串的数组，里面有三条信息。

此时我们已经有了数据，那么下一步就是要添加到我们的index页面中。

通过阅读微信小程序的开发文档，在组建选项卡下，我找到了一个组件叫 [scroll-view](https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html) 。从字面意思我们就可以猜到这个是一个可以滚动的视图。可以滚动的意思是说，当我们的内容太多的时候，屏幕无法一次全部展示，这个组件可以让其滚动起来展示更多的内容。小程序的布局文件都是以wxml结尾的，我们打开index.wxml文件，刚才我们已经将内部无用的部分删除，所以我们在内部添加一个scroll-view。结果如下：

```html
<!--index.wxml-->
<view class="container">
 	<scroll-view>
    </scroll-view>
</view>
```

在scroll-view中间，我们需要展示我们的mockMessages。

先来看一下结果代码：

```html
<!--index.wxml-->
<view class="container">
 	<scroll-view>
    <view wx:for="{{mockMessages}}" wx:for-item="item">
    {{item}}
    </view>
  </scroll-view>
</view>
```

我们这里用到了scroll-view，view两个组件。并且用到了小程序的列表渲染语法wx:for。我们在view中使用了wx:for的意义是，这个for循环会循环创建view。而循环的数组就是我们的mockMessages。接下来，如果你和我一样使用的是TypeScript工程，在微信开发者工具上点击Compile按钮。你可能会遇到tsc not found的错误输出。不要担心，在terminal中使用：

```
sudo npm install typescript -g
```

全局安装typescript的编译器。如果你又遇到了npm not found，那你应该去安装[nodejs](https://nodejs.org/en/)了。安装好后再执行上一条命令安装typescript。

此时我们会得到这样一个结果：

![asuka01](/img//asuka01.png)

## ugly!!!

丑爆了！不要担心，我们来美化一下它。



![asuka02](/img//asuka02.png)

## Better

现在看起来就好了很多了。主要的原因是我们为每一条信息都加了一些美化渲染。这里我们加了居中布局，和阴影效果，那么我们去学怎么实现它。打开index.wxss文件，删掉里面的内容，然后将内容改为如下所示：

```css
/**index.wxss**/
.message{
  margin: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60px;
  box-shadow: 3px 3px 3px grey;
}
```

这里我们创建了一个message类，前面的的句点"."是css里定义类的语法。里面加了margin布局，display使用了flex，然后布局选择了居中，最小高度是60px，以及box-shadow加了3px偏量和grey色。

回到index.wxml文件，修改为如下：

```html
<!--index.wxml-->
<view>
  <scroll-view>
    <view class="message" wx:for="{{mockMessages}}" wx:for-item="item">
    {{item}}
    </view>
  </scroll-view>
</view>
```

这里我们注意到，我们在view内加入了class="message"，并且把最外层的view里的class="container"删除掉了。修改后变可以得到我们的效果样式。

下一步就是增加一个输入框了。

在index.wxml中新增一个input组件：

```html
<!--index.wxml-->
<view>
  <scroll-view>
    <view class="message" wx:for="{{mockMessages}}" wx:for-item="item">
    {{item}}
    </view>
  </scroll-view>
  <input class='input' placeholder='type...'></input>
</view>

```

可以看到我们还为input选择了input类，所以我们在index.wxss中新增input类：

```css
/**index.wxss**/
.message{
  margin: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60px;
  box-shadow: 3px 3px 3px grey;
}

.input {
  margin: 20px;
}
```

结果如下：

![asuka03](/img/asuka03.png)

结果还不错，但是还不够，因为我们希望这个type输入框是在底部的。所以我们还要调整一下.input类的设置。修改代码如下：



```css
/**index.wxss**/
.message{
  margin: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60px;
  box-shadow: 3px 3px 3px grey;
}

.input {
  position: fixed;
  padding: 20px;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: white;
}
```



结果如下：

![asuka04](/img/asuka04.png)



此时我们还需要一个按钮了。该怎么添加呢？

首先请看index.wxml的代码：



```html
<!--index.wxml-->
<view>
  <scroll-view>
    <view class="message" wx:for="{{mockMessages}}" wx:for-item="item">
    {{item}}
    </view>
  </scroll-view>
  <view class='footer'>
    <input class='input' placeholder='type...'/>
    <button class='send-button' size='mini'>SEND</button>
  </view>
</view>

```

我们不仅添加了一个button组件，我们还在input以及button组件的外围添加了一层view组件，对input和button进行了包裹。并且创建了一个footer class来定义view的样式。button也有自己的send-button class来定义自己的样式。我们再去到index.wxss：

```css
/**index.wxss**/
.message{
  margin: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60px;
  box-shadow: 3px 3px 3px grey;
}

.footer {
  position: fixed;
  padding: 10px;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: yellow;
  display: flex;
  justify-content: center;
  align-items: center;
}

.send-button {
  margin: 5px;
}
.input {
  width: 60%;
}
```

最终我们可以看到如下结果：

![asuka05](/img/asuka05.png)



这样，Asuka的UI布局，整体就完成了！看着还不错，学到了吗？有问题可以留言。