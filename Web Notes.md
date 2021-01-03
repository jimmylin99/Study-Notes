# Vue.js

* 响应式

  ##### DOM

  * 与数据建立了关联
    * 文本、attribute、结构（`v-if`, `v-else`, `v-else-if`, `v-for`）
    * 过渡效果

* `F12` 控制台

* `v-model` 双向绑定

* `v-on:<event>="<method_function>"` 事件绑定

* 仅供学习使用的`<head>`引入： 

  * ```html
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
    ```

#### 组件

```vue
<div id="app-5">
    <ol>
        <todo-item 
                   v-for='item in groceryList'
                   v-bind:text='item.text'
                   v-bind:key='item.id'>
        </todo-item>
    </ol>
</div>
<script>
    # register component
    Vue.component('todo-item', {
        props: ['text'],
        template: '<li>{{ text }}</li>'
    })
    var app5 = new Vue({
        el: '#app-5',
        data: {
            groceryList: [
                {id: 0, text: 'Onion'},
                {id: 1, text: 'Ginger'},
                {id: 2, text: 'Lamp'},
                {id: 3, text: 'Sprite'}
            ]
        }
    });
</script>
```

---

# JavaScript

# HTML

##### `<script>`

遇到此标记会停止文档解析，直到脚本从网络抓取且执行完毕

* `defer`关键字
* 异步标记
* 部分浏览器对此直接做了优化

##### Network - Waterfall: chrome 调试工具

* Chrome 并发连接数上限为6

  > browser only allows six TCP connections per origin on **HTTP 1**.

* > 对CSS文件进行压缩处理、使用 CDN，将资源分布在多个域名下、合并 CSS 文件，减少 HTTP 请求数量等，来提高 CSS 的加载速度，减少 HTML 文档解析和渲染的阻塞时间
  >
  > 浏览器的并发请求数目限制是针对同一域名的。因此可以使用 CDN 加速技术来提高用户访问网站的响应速度，这样使用了 CDN 的资源加载不会占用当前域名下的并发连接数，从而减少阻塞的时间。

  Link: [浏览器解析渲染HTML文档的过程_个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000018652029)

* CDN: Content Delivery Network

