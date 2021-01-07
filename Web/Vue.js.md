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

* 响应式

  ##### DOM (详见后文DOM)

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

### 组件

```html
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