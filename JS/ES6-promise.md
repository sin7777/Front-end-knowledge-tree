# promise相应的操作

> 要把常用的方法写上哇，要是面试让你现场写你就凉了哇

## 常见异步面试题

```JS
function sleep() {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log('finish')
      resolve("sleep");
    }, 2000);
  });
}
async function test() {
  let value = await sleep();
  console.log("object");
}
test()

//输出： finish  object
//因为 await 会等待 sleep 函数 resolve ，所以即使后面是同步代码，也不会先去执行同步代码再来执行异步代码。
```

## 模拟实现promise

> [模拟实现promise](http://caibaojian.com/interview-map/frontend/#promise-%E5%AE%9E%E7%8E%B0)
> 感jio比较难，还没有详细看
