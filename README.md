# intopdf

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

# html2canvas
  
  功能：将网页截图 （HTML 转成图片）

  原理：脚本遍历它加载的页面的DOM，它收集所有元素的信息，然后用它来构建页面的，Dom节点在Canvas里边画出来

  因此，它只能正确地呈现它理解的属性，这意味着有许多CSS属性不起作用。有关支持的CSS属性的完整列表，请查看 支持的功能页面

 

  浏览器兼容性：

  Firefox 3.5+
  Google Chrome
  Opera 12+
  IE9+
  Edge
  Safari 6+
  限制： 

  不支持iframe
  不支持跨域图片
  不能在浏览器插件中使用
  部分浏览器上不支持SVG图片
  不支持Flash
  如果你想确认是否支持某个浏览器，可以用它访问 http://deerface.sinaapp.com/ 试试 )

  文档： https://html2canvas.hertzen.com/

# jsPDF
  功能：jsPDF支持在静态网页中直接将html代码转为pdf文件

  限制：
  1. 打印出来PDF不会保留原有的样式
  2. 不支持中文字符，虽然可以通过addhtml的方式来变相实现，但转出来的PDF文件中所有内容都是图片，分辨率并不高
  3. 默认不支持分页；尽管提供了分页的选项，但分页效果真的很差。
  浏览器兼容：

  浏览器支持HTML5就可以 低版本的IE浏览器不支持

  文档：http://rawgit.com/MrRio/jsPDF/master/docs/global.html#addHTML

# html2canvas和jsPDF 结合  实现 HTML转pdf 

安装 html2canvas：n

  npm install --save html2canvas

安装 jsPDF ：

  npm install jspdf --save

使用：
```
<template>
  <div class="hello">
    <button @click="save">点击</button>    
    <h1>{{ msg }}</h1>
      <h2>Essential Links</h2>
      <ul>
        <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
        <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
        <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
        <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
        <br>
        <li><a href="http://vuejs-templates.github.io/webpack/" target="_blank">Docs for This Template</a></li>
      </ul>
  </div>
</template>
 
<script>
import html2canvas from 'html2canvas'
import JsPDF from 'jspdf'
 
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
   // 实现HTML转PDF的方法
   save() {
        html2canvas(document.body).then(canvas => {
          document.body.appendChild(canvas)
          let pageData = canvas.toDataURL('image/jpeg', 1.0)
          // 方向默认竖直，尺寸ponits，格式a4[595.28,841.89]
          let pdf = new JsPDF('', 'pt', 'a4')
          // addImage后两个参数控制添加图片的尺寸，此处将页面高度按照a4纸宽高比列进行压缩
          pdf.addImage(pageData, 'JPEG', 0, 0, 595.28, 592.28 / canvas.width * canvas.height)
          pdf.save('stone.pdf')
        })
    }
  }
}
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
```