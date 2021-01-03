---
title: FontAwesome图标显示小方块
date: 2020-04-14 21:09:39
tags: [FontAwesome,MDI,Vue,Vuetify]
categories: [遇到的问题,前端开发,Vuetify]
copyright: true
---

因为之前玩JQuery的时候用的是FontAwesome的图标包，所以这次用Vuetify重写主页用的也是FontAwesome。一开始也都很顺利，home、traffic-light、graduation-cap、birthday-cake等图标的显示都没有任何问题，但是等用到了QQ和steam等品牌图标的时候就出现了问题。并不是不显示或者直接显示文字，而是出现了方框：
<!--more-->
> 类似于这个☐

在网上查了一些资料也都没有找到对应的解决方法。有些说是css文件对字体的引用不完全导致的问题，但我引用的是all.css而非fontawesome.js

```javascript
import '@fortawesome/fontawesome-free/css/all.css'
```

照理来说应该不会出现没有引入字体的问题。打开all.css也找到了fa-qq和fa-steam，所以应该是字体文件的问题，但后来又尝试把FontAwesome从5.13换成4.7，但仍然是同样的问题。

由于时间不多，因此也没有深入地探究。

解决方法很简单粗暴，就是把出现方块的图标包换成了MDI（因为引用的图标没多少，而且两者内容差不多），意外地发现MDI的图标相比FontAwesome似乎更为小巧精致，更加好看些，索性就全部替换成MDI了。

```javascript
// import '@fortawesome/fontawesome-free/css/all.css'
import '@mdi/font/css/materialdesignicons.css'
Vue.use(Vuetify)
const opts = {
  icons: {
    iconfont: 'mdi'
  }
}

```

等有空了再研究研究FontAwesome吧，毕竟以后应该还是会用到的。这个问题暂时先存着233