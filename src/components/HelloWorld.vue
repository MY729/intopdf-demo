<template>
  <div class="hello" ref="helloword">
    <button @click="intoPdf">点击生成pdf</button>     
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
    intoPdf () {
      html2canvas(this.$refs.helloword).then(canvas => {
        console.log(this.$refs.helloword)
        document.body.appendChild(canvas)
        // 将图片地址生成base码
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
