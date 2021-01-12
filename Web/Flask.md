Great Tutorial: [The Flask Mega-Tutorial Part I: Hello, World! - miguelgrinberg.com](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)

#### Python Import

* The directory for the initial script run by `python` will be added to the `sys.path` automatically

  * `python xxx.py` 直接运行：直接启动是把xx.py文件所在的目录放到了`sys.path`属性中
  * `python -m xxx.py` 把模块当作脚本来启动(注意：但是__name__的值为'main' )：模块启动是把你输入命令的目录（也就是当前路径），放到了`sys.path`属性中

  More: [Python sys.path详解 - 简书 (jianshu.com)](https://www.jianshu.com/p/04701cb81e38)

#### `WTForms`

* `WTForms` is a flexible forms validation and rendering library for Python web development

* `wtforms.validators`

  More: [WTForms — WTForms Documentation (2.3.x)](https://wtforms.readthedocs.io/en/2.3.x/)

#### `Flask-WTF`

###### 防止CSRF

`FlaskForm.hidden_tag()`防止在`form`中，同时在`app.config`中定义`SECRET_KEY`

#### TODO: 

#### Terminology

##### CSRF (Cross-site request forgery) 跨站请求伪造

* 攻击者盗用了你的身份，以你的名义发送恶意请求 

##### Werkzeug

German noun: "tool". It is a comprehensive WSGI web application library

##### WSGI

##### SPHIX

Sphinx is a tool that makes it easy to create intelligent and beautiful documentation, written by Georg Brandl and licensed under the BSD license.



