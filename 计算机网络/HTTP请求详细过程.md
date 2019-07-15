# HTTP请求的全过程

## 请求过程

* 输入url
* 域名解析
* 与服务器建立连接
* 发起HTTP请求
* 服务器响应HTTP请求，浏览器得到html代码
* 浏览器解析html代码，并请求html代码中的资源（如js、css、图片）
* 浏览器对页面进行渲染呈现给用户

## 网页渲染的过程

* 用户输入url
* 获取相应对应的资源
* 渲染引擎调用资源加载器加载相应资源
* HTML解析器生成DOM树
* JavaScript代码由JavaScript引擎处理
* CSS解析出Style Rules（生成CSSOM）
* DOM树建立后根据CSS样式进行构建内部绘图模型，生成RenderObject树
* 根据网页层次结构构建RenderLayer树，同时构建虚拟绘图上下文
* 依赖2D和3D图形库渲染成图像结果呈现在浏览器中（Painting）
  