# 设计模式

## MVC模式

![MVC](../images/MVC.jpg)

## React+Redux中的MVC

react作为视图展示
使用redux注册一个model（放数据的容器）
react只做View层展示，Component通过 redux 的connect链接代码仓库取数数据，在Component内部从props取数展示。
用来redux之后，Component 中的state，一般只做View控制状态的状态码的储存。
Redux API作为Model、Control层
Redux里的store里的state表示Model
Redux里的action和reducer表示Controller

## MVVM模式

双向绑定mvvm

**Model 层**: 对应数据层的域模型，它主要做域模型的同步。通过 Ajax/fetch 等 API 完成客户端和服务端业务 Model 的同步。在层间关系里，它主要用于抽象出 ViewModel 中视图的 Model。

**View 层**:作为视图模板存在，在 MVVM 里，整个 View 是一个动态模板。除了定义结构、布局外，它展示的是 ViewModel 层的数据和状态。View 层不负责处理状态，View 层做的是数据绑定的声明、 指令的声明、 事件绑定的声明。

**ViewModel 层**:把 View 需要的层数据暴露，并对 View 层的 数据绑定声明、 指令声明、 事件绑定声明 负责，也就是处理 View 层的具体业务逻辑。
ViewModel 底层会做好绑定属性的监听。当 ViewModel 中数据变化，View 层会得到更新；而当 View 中声明了数据的双向绑定（通常是表单元素），框架也会监听 View 层（表单）值的变化。一旦值变化，View 层绑定的 ViewModel 中的数据也会得到自动更新。

![MVVM](../images/MVVM.png)
![MVVM](https://user-gold-cdn.xitu.io/2019/8/1/16c498ca0de66530?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

MVVM的优缺点?

优点:

* 分离视图（View）和模型（Model）,**降低代码耦合**，提高视图或者逻辑的重用性: 比如视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定不同的"View"上，当View变化的时候Model不可以不变，当Model变化的时候View也可以不变。你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑
* 提高可测试性: ViewModel的存在可以帮助开发者更好地编写测试代码
* 自动更新dom: 利用双向绑定，数据更新后视图自动更新，让开发者从繁琐的手动dom中解放

缺点:

* Bug很难被调试: 因为使用双向绑定的模式，当你看到界面异常了，有可能是你 View 的代码有 Bug，也可能是 Model 的代码有问题。数据绑定使得一个位置的Bug被快速传递到别的位置，要定位原始出问题的地方就变得不那么容易了。另外，数据绑定的声明是指令式地写在 View 的模版当中的，这些内容是没办法去打断点 debug 的
* 一个大的模块中 model 也会很大，虽然使用方便了也很容易保证了数据的一致性，当时长期持有，不释放内存就造成了花费更多的内存
* 对于大型的图形应用程序，视图状态较多，ViewModel 的构建和维护的成本都会比较高
