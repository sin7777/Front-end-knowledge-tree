# babel 原理

babel实际上类似一般的的语言编译器

babel转换代码的过程主要为三步：

* 解析：输入的javascript代码字符串根据ESTree规范生成AST（抽象语法树）
* 转换：根据一定的规则转换、修改AST
* 生成：使用babel-generator将修改后的AST转换成普通代码
