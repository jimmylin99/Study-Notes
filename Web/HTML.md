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

##### `<label>`

* `for`属性用来关联文档中第一个`id`值与之相同的元素

* 点击或者轻触（tap）与表单控件相关联的 `<label>` 也可以触发关联控件的 `click` 事件

* 另外，你也可以将 `<input>` 直接放在 `<label>` 里，此时则不需要 `for` 和 `id` 属性，因为关联已隐含存在

  ```html
  <label>Do you like peas?
    <input type="checkbox" name="peas">
  </label>
  ```

* 不要在 `label` 元素内部放置可交互的元素，比如 [anchors](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a) 或 [buttons](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button)。这样做会让用户更难激活/触发与 `label` 相关联的表单输入元素。

  * Don't

    ```html
    <label for="tac">
      <input id="tac" type="checkbox" name="terms-and-conditions">
      I agree to the <a href="terms-and-conditions.html">Terms and Conditions</a>
    </label>
    ```

  * Do

    ```html
    <label for="tac">
      <input id="tac" type="checkbox" name="terms-and-conditions">
      I agree to the Terms and Conditions
    </label>
    <p>
      <a href="terms-and-conditions.html">Read our Terms and Conditions</a>
    </p>
    ```

##### `<input>`

##### 声明`HTML5`

```html
<!DOCTYPE html>
```

##### `<meta>`

* `charset`: 'utf-8' (case-insensitive)

###### `viewport meta`

> 窄屏幕设备（如移动设备）在一个虚拟窗口或视口中渲染页面，这个窗口或视口通常比屏幕宽；然后缩小渲染的结果，以便在一屏内显示所有内容。然后用户可以移动、缩放以查看页面的不同区域。例如，如果移动屏幕的宽度为640px，则可能会用980px的虚拟视口渲染页面，然后缩小页面以适应640px的窗口大小。
>
> 这样做是因为许多页面没有做移动端优化，在小窗口渲染时会乱掉（或看起来乱）。所以，这种虚拟视口是一种让未做移动端优化的网站在窄屏设备上看起来更好的办法。
>
> 但是，对于用媒体查询针对窄屏幕做了优化的页面，这种机制不大好 - 比如如果虚拟视口宽 980px，那么在 640px 或 480px 或更小宽度要起作用的媒体查询就不会触发了，浪费了这些响应式设计。

所以引入`viewport meta`

`<meta name='viewport' content='width=device-width, initial-scale=1, maximum-scale=1'>`

> 如果显示设备的像素密度与典型的电脑显示器差异明显，用户代理应当重新调整像素值。推荐的做法是，将一个像素单位（pixel unit）对应为特定数量的设备像素（device pixel），使这个数量的设备像素所占的大小最接近参考像素的大小。而参考像素（reference pixel）的大小则推荐为阅读者在一臂远的距离观看像素密度为 96 dpi 的设备时一个像素所占的视角大小。

> 对于设置了初始或最大缩放的页面，width属性实际上变成了**最小**视口宽度。比如，如果你的布局需要至少500像素的宽度，那么你可以使用以下标记。当屏幕宽度大于500像素时，浏览器会扩展视口（而不是放大页面）来适应屏幕：
>
> ```html
> <meta name="viewport" content="width=500, initial-scale=1">
> ```

More: [在移动浏览器中使用viewport meta标签控制布局 - Mobile | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Mobile/Viewport_meta_tag)

