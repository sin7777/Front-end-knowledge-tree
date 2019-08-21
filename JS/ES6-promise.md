# promise 相应的操作

目录

- [Promise 的理解](#Promise的理解)
- [Promis 的常见 API](#Promis的常见API)
- [常见异步面试题](#常见异步面试题)
- [模拟实现 promise](#模拟实现promise)
- [Promise 实现 ajax](#Promise实现ajax)

## 异步编程的发展过程

事件回调 -> 事件监听 -> Promise -> async/await

## Promise 的理解

Promise 是异步编程的一种解决方案，解决了回调地狱的问题，比传统的异步解决方案【回调函数】和【事件】更合理、更强大。

## Promis 的常见 API

- Promise.resolve(value)
- Promise.reject
- Promise.prototype.then
- Promise.prototype.catch
- Promise.race
- Promise.all

一个 Promise 对象有三个状态，并且状态一旦改变，便不能再被更改为其他状态。

- pending，异步任务正在进行。
- resolved (也可以叫 fulfilled)，异步任务执行成功。
- rejected，异步任务执行失败。

知识点：

- Promise 捕获错误与 try catch 等同
- Promise 拥有状态变化
- Promise 方法中的回调是异步的
- Promise 方法每次都返回一个新的 Promise
- Promise 会存储返回值

## 常见异步面试题

[异步笔试题](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/7)

[Eventloop 不可怕，可怕的是遇上 Promise](https://juejin.im/post/5c9a43175188252d876e5903)

Promise 遇到 resolve 之后才会把 .then 放入微任务队列当中

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
//因为 sleep 函数返回了一个 Promise，之后的代码是挂载在这个 Promise 的 .then 之后，所以会等待 Promise resolve

function sleep() {
  new Promise(resolve => {
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
//输出： object  finish
```

```JS
async function async1() {
    console.log('async1 start');
    await async2();
    setTimeout(function() {
        console.log('setTimeout1')
    },0)
    console.log("async1 end")
}
async function async2() {
  return new Promise(resolve => {
    setTimeout(function() {
      console.log('setTimeout2')
      resolve("async2"); //如果没有resolve则async1不会执行await下面的语句，它会等到await的异步任务resolve
    },1000)
  })
}
//async1 start
//setTimeout2
//async1 end
//setTimeout1


async function async1() {
    console.log('async1 start');
    await async2();
    setTimeout(function() {
        console.log('setTimeout1')
    },0)
}
async function async2() {
  setTimeout(function() {
    console.log('setTimeout2')
  },0)
}
console.log('script start');

setTimeout(function() {
    console.log('setTimeout3');
}, 0)
async1();

new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
});
console.log('script end');

/*
script start
async1 start
promise1
script end
promise2
setTimeout3
setTimeout2
setTimeout1
*/
```

```JS
//红灯三秒亮一次，绿灯一秒亮一次，黄灯2秒亮一次；如何让三个灯不断交替重复亮灯？
function red() {
    console.log('red');
}
function green() {
    console.log('green');
}
function yellow() {
    console.log('yellow');
}

var light = function (timmer, cb) {
    return new Promise(function (resolve, reject) {
        setTimeout(function() {
            cb();
            resolve();
        }, timmer);
    });
};

var step = function() {
    Promise.resolve().then(function () {
        return light(3000, red);
    }).then(function () {
        return light(2000, green);
    }).then(function () {
        return light(1000, yellow);
    }).then(function () {
        step();
    });
}
step();

//阿里面试题，实现函数mergePromise，是异步顺序输出
// 要求分别输出
// 1
// 2
// 3
// done
// [1, 2, 3]
const timeout = ms => new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve();
    }, ms);
});
const ajax1 = () => timeout(2000).then(() => {
    console.log('1');
    return 1;
});
const ajax2 = () => timeout(1000).then(() => {
    console.log('2');
    return 2;
})
const ajax3 = () => timeout(2000).then(() => {
    console.log('3');
    return 3;
})
const mergePromise = ajaxArray => {
  /// 保存数组中的函数执行后的结果
  var data = []
  // Promise.resolve方法调用时不带参数，直接返回一个resolved状态的 Promise 对象。
  var sequence = Promise.resolve();
  ajaxArray.forEach(function (item) {
  // 第一次的 then 方法用来执行数组中的每个函数，
  // 第二次的 then 方法接受数组中的函数执行后返回的结果，
  // 并把结果添加到 data 中，然后把 data 返回。
  // 这里对 sequence 的重新赋值，其实是相当于延长了 Promise 链
    sequence = sequence.then(item).then(function(res) {
        data.push(res);
        return data;
    });
  }
  // 遍历结束后，返回一个 Promise，也就是 sequence， 他的 [[PromiseValue]] 值就是 data，
  // 而 data（保存数组中的函数执行后的结果） 也会作为参数，传入下次调用的 then 方法中。
  return sequence
};
mergePromise([ajax1, ajax2, ajax3]).then(data => {
    console.log('done');
    console.log(data); // data 为 [1, 2, 3]
});

/////////////// 方法每次都返回一个新的Promise
var p1 = new Promise(function(resolve, reject) {
  reject(1);
}).then(
  res => {
    console.log(res);
    return 2;
  },
  err => {
    console.log(err); //输出
    return 3;
  }
).catch(err => {
  console.log(err);
  return 4;
}).finally(res => {
  console.log(res);  //输出
  return 5;
}).then(
  res => console.log(res),  //输出
  err => console.log(err)
);
//1,undefined,3
//finally方法的回调函数不接受任何参数
```

- [加载图片，每次 3 个](https://segmentfault.com/a/1190000016848192)
- [面试官眼中的 Promise,Promise 方法每个都会返回一个 Promise](https://juejin.im/post/5c233a8ee51d450d5a01b712#heading-3)
- [promise-polyfill](https://github.com/taylorhakes/promise-polyfill/blob/master/src/index.js)

## 模拟实现 promise

> [模拟实现 promise](https://zhuanlan.zhihu.com/p/21834559)

先写一个简单版本的

```JS
//基本架构
function Promise(executor) {
  var self = this
  self.status = 'pending' // Promise当前的状态
  self.data = undefined  // Promise的值
  self.onResolvedCallback = [] // Promise resolve时的回调函数集，因为在Promise结束之前有可能有多个回调添加到它上面
  self.onRejectedCallback = [] // Promise reject时的回调函数集，因为在Promise结束之前有可能有多个回调添加到它上面

  executor(resolve, reject) // 执行executor并传入相应的参数
}

//实现版本
function Promise(executor) {
  var self = this
  self.status = 'pending' // Promise当前的状态
  self.data = undefined  // Promise的值
  self.onResolvedCallback = [] // Promise resolve时的回调函数集，因为在Promise结束之前有可能有多个回调添加到它上面
  self.onRejectedCallback = [] // Promise reject时的回调函数集，因为在Promise结束之前有可能有多个回调添加到它上面

  function resolve(value) {
    if (self.status === 'pending') {
      self.status = 'resolved'
      self.data = value
      for(var i = 0; i < self.onResolvedCallback.length; i++) {
        self.onResolvedCallback[i](value)
      }
    }
  }

  function reject(reason) {
    if (self.status === 'pending') {
      self.status = 'rejected'
      self.data = reason
      for(var i = 0; i < self.onRejectedCallback.length; i++) {
        self.onRejectedCallback[i](reason)
      }
    }
  }

  try { // 考虑到执行executor的过程中有可能出错，所以我们用try/catch块给包起来，并且在出错后以catch到的值reject掉这个Promise
    executor(resolve, reject) // 执行executor
  } catch(e) {
    reject(e)
  }
}

Promise.prototype.then = function(onResolved, onRejected) {
  var self = this
  var promise2

  // 根据标准，如果then的参数不是function，则我们需要忽略它，此处以如下方式处理
  onResolved = typeof onResolved === 'function' ? onResolved : function(value) {}
  onRejected = typeof onRejected === 'function' ? onRejected : function(reason) {}

  if (self.status === 'resolved') {
    // 如果promise1(此处即为this/self)的状态已经确定并且是resolved，我们调用onResolved
    // 因为考虑到有可能throw，所以我们将其包在try/catch块里
    return promise2 = new Promise(function(resolve, reject) {
      try {
        var x = onResolved(self.data)
        if (x instanceof Promise) { // 如果onResolved的返回值是一个Promise对象，直接取它的结果做为promise2的结果
          x.then(resolve, reject)
        }
        resolve(x) // 否则，以它的返回值做为promise2的结果
      } catch (e) {
        reject(e) // 如果出错，以捕获到的错误做为promise2的结果
      }
    })
  }

  // 此处与前一个if块的逻辑几乎相同，区别在于所调用的是onRejected函数，就不再做过多解释
  if (self.status === 'rejected') {
    return promise2 = new Promise(function(resolve, reject) {
      try {
        var x = onRejected(self.data)
        if (x instanceof Promise) {
          x.then(resolve, reject)
        }
      } catch (e) {
        reject(e)
      }
    })
  }

  if (self.status === 'pending') {
  // 如果当前的Promise还处于pending状态，我们并不能确定调用onResolved还是onRejected，
  // 只能等到Promise的状态确定后，才能确实如何处理。
  // 所以我们需要把我们的**两种情况**的处理逻辑做为callback放入promise1(此处即this/self)的回调数组里
  // 逻辑本身跟第一个if块内的几乎一致，此处不做过多解释
    return promise2 = new Promise(function(resolve, reject) {
      self.onResolvedCallback.push(function(value) {
        try {
          var x = onResolved(self.data)
          if (x instanceof Promise) {
            x.then(resolve, reject)
          }
        } catch (e) {
          reject(e)
        }
      })

      self.onRejectedCallback.push(function(reason) {
        try {
          var x = onRejected(self.data)
          if (x instanceof Promise) {
            x.then(resolve, reject)
          }
        } catch (e) {
          reject(e)
        }
      })
    })
  }
}

// 为了下文方便，我们顺便实现一个catch方法
Promise.prototype.catch = function(onRejected) {
  return this.then(null, onRejected)
}

```

## Promise 实现 ajax

```JS
const $ = (function() {
  const ajax = function(url, async = false, type = "GET") {
    const promise = new Promise(function(resolve, reject) {
      const handler = function() {
        if (this.readyState !== 4) {
          return;
        }
        if (this.status === 200) {
          resolve(this.response);
        } else {
          reject(new Error(this.statusText));
        }
      };
      const client = new XMLHttpRequest();
      client.open(type, url)
      client.onreadystatechange = handler;
      client.responseType = "json";
      client.setRequestHeader("Accept", "application/json")
      client.send();
    })
    return promise;
  }
  return {
    ajax
  };
})();

//调用方式：
$.ajax("/posts.json").then(
  function(json) {
    console.log("Contents: " + json);
  },
  function(error) {
    console.error("出错了", error);
  }
);
```

## Promise 与 setTimeOut

```JS
new Promise(function(resolve) {
  console.log("a");
  for (var i = 0; i < 10000; i++) {
    if (i === 9999) {
      resolve();
    }
  }
  console.log("b");
}).then(function() {
  console.log("c");
});
//a,b,c

new Promise(function(resolve) {
  console.log("a");
  setTimeout(function() {
    console.log("d");
    resolve();
  }, 2000);
  console.log("b");
}).then(function() {
  console.log("c");
});
//a,b,d,c

new Promise(function(resolve) {
  return new Promise(() => {
    console.log("a");
    setTimeout(() => {
      console.log("b");
      resolve("c");
    }, 2000);
  });
}).then(res => {
  console.log(res);
});
//a,b,c

new Promise(function(resolve) {
  return new Promise(resolve => {
    console.log("a");
    setTimeout(() => {
      console.log("b");
      resolve("c");
    }, 2000);
  }).then(res => {
    console.log(res);
    resolve("m");
  });
}).then(res => {
  console.log(res);
});
//a, b, c, m

new Promise(function(resolve) {
  resolve("m");
  return new Promise(resolve => {
    console.log("a");
    setTimeout(() => {
      console.log("b");
      resolve("c");
    }, 2000);
  }).then(res => {
    console.log(res);
  });
}).then(res => {
  console.log(res);
});
//a, m, b, c

new Promise(function(resolve) {
  new Promise(resolve => {
    console.log("a");
    setTimeout(() => {
      console.log("b");
      resolve("c");
    }, 2000);
  }).then(res => {
    console.log(res);
  });
  setTimeout(() => {
    console.log("d");
    resolve("e");
  }, 2000);
}).then(res => {
  console.log(res);
});
//a,b,c,d,e

```
