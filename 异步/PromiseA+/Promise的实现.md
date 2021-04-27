# PromiseA+实现

## 规范重点解读

### 一、 Promise 状态

#### pending 

> 待定，Promise 的初始状态。在此状态下可以`落定 (settled)` 为 `fulfilled` 或 `rejected `状态。

#### fulfilled

> 兑现(解决)，表示执行成功。Promise 被 resolve 后的状态，状态不可再改变，且有一个私有的值 value。

#### pending 

> 待定，Promise 的初始状态。在此状态下可以`落定 (settled)` 为 `fulfilled` 或 `rejected `状态。

### 二 、Then 方法

要求必须提供一个 then 方法来访问当前或最终的 value 或 reason。

- **then 方法接受两个函数作为参数，且参数可选。**
- **如果可选参数不为函数时会被忽略。**
- **两个函数都是异步执行，会放入事件队列等待下一轮 tick。**
- **当调用 onFulfilled 函数时，会将当前 Promise 的 value 值作为参数传入。**
- **当调用 onRejected 函数时，会将当前 Promise 的 reason 失败原因作为参数传入。**
- **then 函数的返回值为 Promise。**
- **then 可以被同一个 Promise 多次调用。**

### 三、Promise 解决过程

Promise 的 resolve 解决过程是一个抽象操作，接收一个 Promise 和一个值 x。

- **x 等于 Promise**
  - 抛出 TypeError 错误，拒绝 Promise。
- **x 是 Promise 的实例**
  - 如果 x 处于待定状态，那么 Promise 继续等待直到 x 兑现或拒绝，否则根据 x 的状态兑现/拒绝 Promise。
- **x 是对象或函数**
  - 取出 x.then 并调用，调用时将 this 指向 x。将 then 回调函数中得到的结果 y 传入新的 Promise 解决过程中，递归调用。
- **如果 x 不是对象或函数**
  - 以 x 作为值执行 Promise。



## **手写 Promise**

### 1.首先定义 Promise 的三个状态

```js
var PENDING = 'pending';
var FULFILLED = 'fulfilled';
var REJECTED = 'rejected';
```

### 2.我们再来搞定 Promise 的构造函数

```js
function Promise(execute) {
    var that = this;
    that.state = PENDING;
    function resolve(value) {
        if (that.state === PENDING) {
            that.state = FULFILLED;
            that.value = value;
        }
    }
    function reject(reason) {
        if (that.state === PENDING) {
            that.state = REJECTED;
            that.reason = reason;
        }
    }
    try {
        execute(resolve, reject);
    } catch (e) {
        reject(e);
    }
}
```

### 3. 实现 then() 方法

`then` 方法用来注册当前 Promise 状态落定后的回调，每个 Promise 实例都需要有它，显然要写到 Promise 的原型 `prototype` 上，并且 then() 函数接收两个回调函数作为参数，分别是 `onFulfilled` 和 `onRejected`。

```js
// 1.then() 函数接收两个回调函数作为参数 onFulfilled、onRejected
Promise.prototype.then = function (onFulfilled, onRejected) {
  // 2.如果可选参数不为函数时应该被忽略，我们要对参数进行如下判断。
  onFulfilled =
    typeof onFulfilled === "function"
      ? onFulfilled
      : function (x) {
          return x;
        };
  onRejected =
    typeof onRejected === "function"
      ? onRejected
      : function (e) {
          throw e;
        };

  var that = this;
  var promise;

  // 根据第 4 条、第 5 条规则，需要根据 Promise 的状态来执行对应的回调函数
  if (that.state === FULFILLED) {
    // then 函数的返回值为 Promise，我们分别给每个逻辑添加并返回一个 Promise。
    promise = new Promise(function (resolve, reject) {
      // 根据第 3 条规则，需要使用 setTimeout 延迟执行，模拟异步
      setTimeout(function () {
        try {
          onFulfilled(that.value);
        } catch (reason) {
          reject(reason);
        }
      });
    });
  }
  if (that.state === REJECTED) {
    // then 函数的返回值为 Promise，我们分别给每个逻辑添加并返回一个 Promise。
    promise = new Promise(function (resolve, reject) {
      // 根据第 3 条规则，需要使用 setTimeout 延迟执行，模拟异步
      setTimeout(function () {
        try {
          onRejected(that.reason);
        } catch (reason) {
          reject(reason);
        }
      });
    });
  }
  if (that.state === PENDING) {
    // then 函数的返回值为 Promise，我们分别给每个逻辑添加并返回一个 Promise。
    promise = new Promise(function (resolve, reject) {
      // then 支持链式调用，我们需要将 onFulfilledFn 和 onRejectedFn 改成数组。
      that.onFulfilledFn.push(function () {
        try {
          onFulfilled(that.value);
        } catch (reason) {
          reject(reason);
        }
      });
      // then 支持链式调用，我们需要将 onFulfilledFn 和 onRejectedFn 改成数组。
      that.onRejectedFn.push(function () {
        try {
          onRejected(that.reason);
        } catch (reason) {
          reject(reason);
        }
      });
    });
  }
};

```

再将 Promise 的构造函数相应的进行改造。

```js
function Promise(execute) {
    var that = this;
    that.state = PENDING;
    that.onFulfilledFn = [];
    that.onRejectedFn = [];

    function resolve(value) {
        setTimeout(function() {
            if (that.state === PENDING) {
                that.state = FULFILLED;
                that.value = value;
                that.onFulfilledFn.forEach(function(fn) {
                    fn(that.value);
                })
            }
        })
    }
    function reject(reason) {
        setTimeout(function() {
            if (that.state === PENDING) {
                that.state = REJECTED;
                that.reason = reason;
                that.onRejectedFn.forEach(function(fn) {
                    fn(that.reason);
                })
            }
        })
    }
    try {
        execute(resolve, reject);
    } catch (e) {
        reject(e);
    }
}

```

### 4.Promise 解决过程 resolvePromise()

**1.x 等于 Promise TypeError 错误**

此时相当于 Promise.then 之后 return 了自己，因为 then 会等待 return 后的 Promise，导致自己等待自己，一直处于等待。。

```js
function resolvePromise(promise, x) {
    if (promise === x) {
        return reject(new TypeError('x 不能等于 promise'));
    }
}
```

**2.x 是 Promise 的实例**

如果 x 处于待定状态，Promise 会继续等待直到 x 兑现或拒绝，否则根据 x 的状态兑现/拒绝 Promise。

我们需要调用 Promise 在构造时的函数 resolve() 和 reject() 来改变 Promise 的状态。

```js
function resolvePromise(promise, x, resolve, reject) {
    // ...
    if (x instanceof Promise) {
        if (x.state === FULFILLED) {
            resolve(x.value);
        } else if (x.state === REJECTED) {
            reject(x.reason);
        } else {
            x.then(function(y) {
                resolvePromise(promise, y, resolve, reject);
            }, reject);
        }
    }
}
```

**3.x 是对象或函数**

取出 x.then 并调用，调用时将 this 指向 x，将 then 回调函数中得到的结果 y 传入新的 Promise 解决过程中，递归调用。

如果执行报错，则将以对应的失败原因拒绝 Promise。

x 可能是一个 thenable 而非真正的 Promise。

需要设置一个变量 `executed` 避免重复调用。

```js
function resolvePromise(promise, x, resolve, reject) {
    // ...
    if ((x !== null) && ((typeof x === 'object' || (typeof x === 'function'))) {
        var executed;
        try {
            var then = x.then;
            if (typeof then === 'function') {
                then.call(x, function(y) {
                    if (executed) return;
                    executed = true;
                    return resolvePromise(promise, y, resolve, reject);
                }, function (e) {
                    if (executed) return;
                    executed = true;
                    reject(e);
                }) 
            } else {
                resolve(x);
            }
        } catch (e) {
            if (executed) return;
            executed = true;
            reject(e);
        }
    }
}
```

**4.直接将 x 作为值执行**

```js
function resolvePromise(promise, x, resolve, reject) {
    // ...
    resolve(x)
}
```

## 完整实现

```js
var PENDING = 'pending';
var FULFILLED = 'fulfilled';
var REJECTED = 'rejected';

function Promise(execute) {
  var that = this;
  that.state = PENDING;
  that.value = null;
  that.reason = null
  that.onFulfilledFn = [];
  that.onRejectedFn = [];

  function resolve(value) {
    setTimeout(function () {
      if (that.state === PENDING) {
        that.state = FULFILLED;
        that.value = value;
        that.onFulfilledFn.forEach(function (f) {
          f(that.value);
        });
      }
    });
  }

  function reject(reason) {
    setTimeout(function () {
      if (that.state === PENDING) {
        that.state = REJECTED;
        that.reason = reason;
        that.onRejectedFn.forEach(function (f) {
          f(that.reason);
        });
      }
    });
  }
  try {
    execute(resolve, reject);
  } catch (e) {
    reject(e);
  }
}
Promise.prototype.then = function (onFulfilled, onRejected) {
  onFulfilled =
    typeof onFulfilled === 'function'
      ? onFulfilled
      : function (x) {
        return x;
      };
  onRejected =
    typeof onRejected === 'function'
      ? onRejected
      : function (e) {
        throw e;
      };
  var that = this;
  var promise;
  if (that.state === FULFILLED) {
    promise = new Promise(function (resolve, reject) {
      setTimeout(function () {
        try {
          var x = onFulfilled(that.value);
          resolvePromise(promise, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    });
  }
  if (that.state === REJECTED) {
    promise = new Promise(function (resolve, reject) {
      setTimeout(function () {
        try {
          var x = onRejected(that.reason);
          resolvePromise(promise, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    });
  }
  if (that.state === PENDING) {
    promise = new Promise(function (resolve, reject) {
      that.onFulfilledFn.push(function () {
        try {
          var x = onFulfilled(that.value);
          resolvePromise(promise, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
      that.onRejectedFn.push(function () {
        try {
          var x = onRejected(that.reason);
          resolvePromise(promise, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    });
  }
  return promise;
};

function resolvePromise(promise, x, resolve, reject) {
  if (promise === x) {
    return reject(new TypeError('x 不能与 promise 相等'));
  }
  if (x instanceof Promise) {
    x.then(function (y) {
      return resolvePromise(promise, y, resolve, reject);
    }, reject);
  } else if (x !== null && (typeof x === 'object' || typeof x === 'function')) {
    var executed;
    try {
      var then = x.then;
      if (typeof then === 'function') {
        then.call(
          x,
          function (y) {
            if (executed) return;
            executed = true;
            resolvePromise(promise, y, resolve, reject);
          },
          function (e) {
            if (executed) return;
            executed = true;
            reject(e);
          }
        );
      } else {
        resolve(x);
      }
    } catch (e) {
      if (executed) return;
      executed = true;
      reject(e);
    }
  } else {
    resolve(x);
  }
}

module.exports = {
  deferred() {
    var resolve;
    var reject;
    var promise = new Promise(function (res, rej) {
      resolve = res;
      reject = rej;
    });
    return {
      promise,
      resolve,
      reject,
    }
  }
}
```

