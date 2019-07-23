# Some JS Tips

## 词法作用域与动态作用域

JS采用的是词法作用域，也就是说静态作用域，函数的作用域在函数定义的时候就决定了。

而与词法作用域相对的是动态作用域，函数的作用域是在函数调用的时候才决定的。

## 执行上下文创建之后

执行上下文的生命周期有两个阶段：

* 创建阶段：执行上下文会分别创建变量对象，建立作用域链，以及确定this的指向
* 代码执行阶段：会完成变量赋值，函数引用，以及执行其他代码
  
变量对象会包括:

1. 函数的所有形参(如果是函数上下文)
    由名称和对应值组成的一个变量对象的属性被创建
    没有实参，属性值设为undefined
2. 函数声明
    由名称和对应值(函数对象(function-object)) 组成一个变量对象的属性被创建
    如果变量对象已经存在相同名称的属性，则完全替换这个属性
3. 变量声明
    由名称和对应值(undefined) 组成一 个变量对象的属性被创建;
    如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性

## Object.create

Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__

```JS
Object.create(proto, [propertiesObject])
//proto  新创建对象的原型对象
//propertiesObject  新创建对象的可枚举属性
   //configurable  true当且仅当该属性描述符的类型可以被改变并且该属性可以从对应对象中删除
   //enumerable  true 当且仅当在枚举相应对象上的属性时该属性显现
   //value  
   //writable  true 当且仅当与该属性相关联的值可以用assignment operator改变时


Object.create(null)  //创建的对象是一个空对象,没有继承任何方法
var o = Object.create({})
var p = Object.create(Object.prototype)
```

Object.create() 与 new Object() 的区别
