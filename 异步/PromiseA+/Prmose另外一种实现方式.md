## PromiseA+的实现方式

```js
//Promise/A+规定的三种状态
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'

class MyPromise {
    // 构造方法接收一个回调
    constructor(executor) {
        this._status = PENDING     // Promise状态
        this._value = undefined    // 储存then回调return的值
        this._resolveQueue = []    // 成功队列, resolve时触发
        this._rejectQueue = []     // 失败队列, reject时触发

        // 由于resolve/reject是在executor内部被调用, 因此需要使用箭头函数固定this指向, 否则找不到this._resolveQueue
        let _resolve = (val) => {
            //把resolve执行回调的操作封装成一个函数,放进setTimeout里,以兼容executor是同步代码的情况
            const run = () => {
                if (this._status !== PENDING) return   // 对应规范中的"状态只能由pending到fulfilled或rejected"
                this._status = FULFILLED              // 变更状态
                this._value = val                     // 储存当前value

                // 这里之所以使用一个队列来储存回调,是为了实现规范要求的 "then 方法可以被同一个 promise 调用多次"
                // 如果使用一个变量而非队列来储存回调,那么即使多次p1.then()也只会执行一次回调
                while (this._resolveQueue.length) {
                    const callback = this._resolveQueue.shift()
                    callback(val)
                }
            }
            setTimeout(run)
        }
        // 实现同resolve
        let _reject = (val) => {
            const run = () => {
                if (this._status !== PENDING) return   // 对应规范中的"状态只能由pending到fulfilled或rejected"
                this._status = REJECTED               // 变更状态
                this._value = val                     // 储存当前value
                while (this._rejectQueue.length) {
                    const callback = this._rejectQueue.shift()
                    callback(val)
                }
            }
            setTimeout(run)
        }
        // new Promise()时立即执行executor,并传入resolve和reject
        try {
            executor(_resolve, _reject)
        } catch (error) {
            _reject(error)
        }
    }

    // then方法,接收一个成功的回调和一个失败的回调
    then(resolveFn, rejectFn) {
        // 根据规范，如果then的参数不是function，则我们需要忽略它, 让链式调用继续往下执行
        typeof resolveFn !== 'function' ? resolveFn = value => value : null
        typeof rejectFn !== 'function' ? rejectFn = reason => {
            throw reason;
        } : null

        // return一个新的promise
        const p = new MyPromise((resolve, reject) => {
            // 把resolveFn重新包装一下,再push进resolve执行队列,这是为了能够获取回调的返回值进行分类讨论
            const fulfilledFn = value => {
                try {
                    // 执行第一个(当前的)Promise的成功回调,并获取返回值
                    let x = resolveFn(value)
                    // 分类讨论返回值,如果是Promise,那么等待Promise状态变更,否则直接resolve
                    resolvePromise(p, x, resolve, reject)
                } catch (error) {
                    reject(error)
                }
            }

            // reject同理
            const rejectedFn = error => {
                try {
                    let x = rejectFn(error)
                    resolvePromise(p, x, resolve, reject)
                } catch (error) {
                    reject(error)
                }
            }

            switch (this._status) {
                // 当状态为pending时,把then回调push进resolve/reject执行队列,等待执行
                case PENDING:
                    this._resolveQueue.push(fulfilledFn)
                    this._rejectQueue.push(rejectedFn)
                    break;
                // 当状态已经变为resolve/reject时,直接执行then回调
                case FULFILLED:
                    setTimeout(() => {
                        fulfilledFn(this._value)
                    });
                    break;
                case REJECTED:
                    setTimeout(() => {
                        rejectedFn(this._value)
                    });
                    break;
            }
        })
        return p
    }
}
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
        var promise = new MyPromise(function (res, rej) {
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

