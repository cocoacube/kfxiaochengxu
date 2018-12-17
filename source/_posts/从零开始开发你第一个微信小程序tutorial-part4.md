---
uuid: 0cd7e430-01e3-11e9-b145-bbedaa2ac6a6
title: 从零开始开发你第一个微信小程序tutorial-part4
date: 2018-12-17 18:03:49
tags:
---

上一期介绍了BaaS服务，使用BaaS服务可以让小程序的功能更加强大，并且降低开发难度和成本。我们这一期就来介绍一下如何通过BaaS来完成我们Asuka小程序的最后一步！

我们进入[leancloud.cn][https://leancloud.cn/docs/rest_api.html]的Rest API部分。Rest API是一种常用到的通信接口，通过HTTP请求就可以获取、查询修改数据库中的内容。而我们用到leancloud中的主要功能就是Leancloud的云端数据库。在微信小程序中，微信提供了一个可以发送Http请求的API  *wx.request* 。通过leancloud的Rest API和微信的request API接口，我们可以实现对leancloud云端数据库的操作。

首先我们在index.ts文件中新增一个方法：*createMessage* 

```javascript
//index.js
//获取应用实例
import { IMyApp } from '../../app'

const app = getApp<IMyApp>()

Page({
  data: {
    mockMessages: ['Hello Asuka!', 'I am asuka user1', 'Who is it there?', 'I am asuka user1', 'Who is it there?', 'I am asuka user1', 'Who is it there?', 'I am asuka user1', 'Who is it there?']
  },
  //事件处理函数
  bindViewTap() {
    wx.navigateTo({
      url: '../logs/logs'
    })
  },
  onLoad() {
  },

  getUserInfo(e: any) {
    console.log(e)
    app.globalData.userInfo = e.detail.userInfo
    this.setData!({
      userInfo: e.detail.userInfo,
      hasUserInfo: true
    })
  },
    
   // Create Message 方法，用来创建要发送的信息 
   createMessage() {
   	    
   }
})

```

在*createMessage*方法中，我们需要调用微信的*wx.request* API。我们可以查看官方的[使用文档][https://developers.weixin.qq.com/miniprogram/dev/api/wx.request.html] 。

```javascript
wx.request({
  url: 'test.php', // 仅为示例，并非真实的接口地址
  data: {
    x: '',
    y: ''
  },
  header: {
    'content-type': 'application/json' // 默认值
  },
  success(res) {
    console.log(res.data)
  }
})
```

按照微信官方给出的示例代码，我们可以修改我们的*createMessage*方法。修改后的方法如下：

```javascript
createMessage() {
   	 wx.request({
      url: 'test.php', // 仅为示例，并非真实的接口地址
      data: {
        x: '',
        y: ''
      },
      header: {
        'content-type': 'application/json' // 默认值
      },
      success(res) {
        console.log(res.data)
      }
    })   
}
```

只是把代码粘贴进去还是远远不够的，我们需要对request API进行修改，比如修改url到leancloud的URL，修改data，修改header设置。如何设置我们需要参考leancloud的[RestAPI文档][https://leancloud.cn/docs/rest_api.html#hash650308615]

```javascript
curl -X POST \
  -H "X-LC-Id: xxxxxxxxxxxxx-gzGzoHsz" \
  -H "X-LC-Key: ifhd1dxxxxxxxxxx4hkQVKLU" \
  -H "Content-Type: application/json" \
  -d '{"content": "每个 Java 程序员必备的 8 个开发工具","pubUser": "LeanCloud官方客服","pubTimestamp": 1435541999}' \
  https://4g800ocq.api.lncld.net/1.1/classes/Post
```

上面的示例是经常见到的使用curl发送http请求的方法，-X 后面的 POST 表示这是一个POST请求，-H代表的是Header内的配置，-d表示的是data，最后面的https就是url地址。在使用的时候，大家需要把X-LC-Id和X-LC-Key的值换成自己leancloud小程序的对应id和key。

我们接着完成*createMessage*方法，结果如下：

```javascript
createMessage() {
   	 wx.request({
      url: 'https://4g800ocq.api.lncld.net/1.1/classes/Post', 
      data: {
        message: 'This is a message test!'
      },
      header: {
        'content-type': 'application/json',
          'X-LC-Id': '4g800OCQvSQPKfJhvKowbpeI-gzGzoHsz',
          'X-LC-Key': 'ifhd1dhKzd6WfmKH4hkQVKLU'
      },
      success(res) {
        console.log(res.data)
      }
    })   
}
```

然后将这个createMessage()方法和SEND按钮进行绑定，这样就可以在按SEND的时候，自动创建一条内容为'This is a message test!'的信息了，并且会存储到leanclou的云端数据库中。

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
    <button class='send-button' size='mini' bindtap='createMessage'>SEND</button>
  </view>
</view>
```

我在测试的时候发现，若发送request请求，需要对域名进行白名单处理和备案。这是很恼火的事情，看来使用leancloud开发小程序还是任重而道远。不过基本思路就是这样，大家可以自己添加白名单继续开发！

