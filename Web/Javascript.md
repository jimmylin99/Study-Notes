### 事件

Reference: [事件介绍 - 学习 Web 开发 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Building_blocks/Events)

###### 事件监听

1. html标签属性（不建议）
2. DOM level 0事件（非正式表述，实际上就是html与js分离的实现方法）（如onclick）
3. DOM level 2事件（如addEventListener, removeEventListener)
   * 优点：可以添加多个同类型事件，可以移除事件

###### 事件对象

事件监听中包含回调函数(callback)（此处具体指事件处理函数(event handler function)），内含参数，常被denote为`e`（即事件对象 event object）

* 事件对象 `e` 的`target`属性始终是事件刚刚发生的元素的引用

###### 阻止默认行为

在事件对象上调用`e.preventDefault()`

###### 事件冒泡及捕获



### 动态绑定

###### 事件绑定顺序

* html属性(html标签中的onclick) > DOM元素属性(onclick；即DOM level 0事件) > DOM level 2 事件(jq的on, click; 原生的addEventListener)

* 同一种绑定事件方式顺序：写在前面 > 后面

* *同一种html属性和 DOM level 0 事件 只有最后一个事件监听器会被绑定（即会覆盖）；DOM level 2 事件会全部绑定而因此全部执行（在监听到事件时）*

* 不建议将`html`属性（即html标签中的事件属性）与`js`一同使用，请做到将编程逻辑与内容分离

* 动态绑定问题：

  * 对于jq或原生js创建的元素，一般发生在所有渲染和脚本执行结束后，所以有些事件绑定将无效，例如监听某`input`元素的`click`事件，行为函数为`document.createElement('li');`, 并修改其`innerHTML`使之包含`id` and `class="test"`, 并添加至已创建的`ul`中，则：
    * `$('.test').on()` 对其无效 （因为该`li`的创建发生在jq查询所有结点'.test'之后，且其父节点们没有'.test'）
    * `$('.test').click()` 也无效，理由同上
    * `$(document).on('click', '.test', func)` 有效，因为任意在DOM中的元素都有祖先节点`document`, 使得在事件上传时会被监听到
      * 但不建议使用`document` 而应该使用其最近父祖先以提升性能

  More: [(2) 【JavaScript系列】动态绑定事件方法：(1)jquery的on方法；(2)html元素绑定_技术记录分享 - SegmentFault 思否](https://segmentfault.com/a/1190000019864627)

###### href="javascript:;"

* equivalent to 

  ```js
  href="javascript:void(0);"
  href="javascript:void:;"
  ```

  表示什么都不执行，用于例如` <a href="javascript:;"  onclick="GetExplorer();">测试一</a> ` 的 `a` tag

### jQuery

* `ready()`

  * ```js
    $(function() {
      // Handler for .ready() called.
    });
    ```

* Event Delegation: `on()`

* Notice that `defer` attribute only effective for `<script src="..." defer>` with `src`

  More: [javascript - Why does jQuery or a DOM method such as getElementById not find the element? - Stack Overflow](https://stackoverflow.com/questions/14028959/why-does-jquery-or-a-dom-method-such-as-getelementbyid-not-find-the-element)

### 箭头函数&匿名函数

> 请注意尽管匿名函数和箭头函数有些类似，但是他们绑定不同的`this`对象。匿名函数（和所有传统的Javascript函数）创建他们独有的`this`对象，而箭头函数则继承绑定他所在函数的`this`对象。
>
> 这意味着在使用箭头函数时，原函数中可用的变量和常量在事件处理器中同样可用。

