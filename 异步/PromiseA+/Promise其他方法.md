# Promise其他方法实现代码



## Promise.resolve

```js
Promise.resolve = function(value) {
    if (value instanceof Promise) {
        return value;
    }
    return new Promise(function(resolve, reject) {
        resolve(value);
    });
}
```



## Promise.reject

```javascript
Promise.reject = function(reason) {
    return new Promise(function(resolve, reject) {
        reject(reason);
    });
}
```



## Promise.prototype.catch

```js
Promise.prototype.catch = function(onRejected) {
    return this.then(null, onRejected);
}
```



## Promise.prototype.finally

```js
Promise.prototype.finally = function(fn) {
    return this.then(function(value) {
        return Promise.resolve(fn()).then(function() {
            return value;
        });
    }, function(error) {
        return Promise.resolve(fn()).then(function() {
            throw error;
        });
    });
}
```



## Promise.all

```js
Promise.all = function(promiseArr) {
    return new Promise(function(resolve, reject) {
        const length = promiseArr.length;
        const result = [];
        let count = 0;
        if (length === 0) {
            return resolve(result);
        }
        for (let [i, p] of promiseArr.entries()) {
            Promise.resolve(p).then(function(data) {
                result[i] = data;
                count++;
                if (count === length) {
                    resolve(result);
                }
            }, function(reason) {
                reject(reason);
            });
        }
    });
}
```



## Promise.race

```js
Promise.race = function(promiseArr) {
    return new Promise(function(resolve, reject) {
        const length = promiseArr.length;
        if (length === 0) {
            return resolve();
        } 

        for (let item of promiseArr) {
            Promise.resolve(item).then(function(value) {
                return resolve(value);
            }, function(reason) {
                return reject(reason);
            });
        }
    });
}
```



## Promise.any

```js
Promise.any = function(promiseArr) {
    return new Promise(function(resolve, reject) {
        const length = promiseArr.length;
        const result = [];
        let count = 0;
        if (length === 0) {
            return resolve(result);
        } 

        for (let [i, p] of promiseArr.entries()) {
            Promise.resolve(p).then((value) => {
                return resolve(value);
            }, (reason) => {
                result[i] = reason;
                count++;
                if (count === length) {
                    reject(result);
                }
            });
        }
    });
}
```



## Promise.allSettled

```js
Promise.allSettled = function(promiseArr) {
  return new Promise(function(resolve) {
    const length = promiseArr.length;
    const result = [];
    let count = 0;

    if (length === 0) {
      return resolve(result);
    } else {
      for (let [i, p] of promiseArr.entries()) {
        Promise.resolve(p).then((value) => {
            result[i] = { status: 'fulfilled', value: value };
            count++;
            if (count === length) {
                return resolve(result);
            }
        }, (reason) => {
            result[i] = { status: 'rejected', reason: reason };
            count++;
            if (count === length) {
                return resolve(result);
            }
        });
      }
    }
  });
}

// 使用 Promise.finally 实现
Promise.allSettled = function(promises) {
    // 也可以使用扩展运算符将 Iterator 转换成数组
    // const promiseArr = [...promises]
    const promiseArr = Array.from(promises)
    return new Promise(resolve => {
        const result = []
        const len = promiseArr.length;
        let count = len;
        if (len === 0) {
          return resolve(result);
        }
        for (let i = 0; i < len; i++) {
            promiseArr[i].then((value) => {
                result[i] = { status: 'fulfilled', value: value };
            }, (reason) => {
                result[i] = { status: 'rejected', reason: reason };
            }).finally(() => { 
                if (!--count) {
                    resolve(result);
                }
            });
        }
    });
}

// 使用 Promise.all 实现
Promise.allSettled = function (promises) {
  // 也可以使用扩展运算符将 Iterator 转换成数组
  // const promiseArr = [...promises]
  const promiseArr = Array.from(promises);
  return Promise.all(
    promiseArr.map((p) =>
      Promise.resolve(p).then(
        (res) => {
          return { status: "fulfilled", value: res };
        },
        (error) => {
          return { status: "rejected", reason: error };
        }
      )
    )
  );
};


```



