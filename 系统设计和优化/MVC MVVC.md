# 设计模式

## MVC模式

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
