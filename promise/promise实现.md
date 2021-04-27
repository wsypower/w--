# promise



## promise A+规范

####  🍅 术语

- promise 一个有then方法的对象或函数，其行为符合本规范
- thenable 一个定义了then方法的对象或函数
- value（值） 任何javaScript的合法值（包括 undefined,thenable或promise）
- exception（异常） throw语句抛出的值
- reason（拒绝原因） 表示promise被拒绝原因的值

####  🍅 promise的状态

- pending：等待,可以转换成fulfilled或rejected状态
- fulfilled：完成，不能迁移到其它状态，拥有一个不可变的终值
- rejected：拒绝，不能迁移到其它状态，拥有一个不可变的据因

#### 🍅 promise的then方法

一个promise必须提供一个then方法以访问最终值value和reason

promise的then方法接受两个参数

```javascript
promise.then(onFulfilled, onRejected)
```

- then 方法的参数
  - 两个函数参数，都是可选参数
  - onFulfilled在promise完成后被调用，onRejected在promise被拒绝执行后调用；onFulfilled和onRejected如果不是函数，其必须被忽略
- then方法的调用：可以调用多次
- then方法的返回值：promise

**1. onFulfilled和onRejected都是可选参数**（参数可选）

- onFulfilled不是一个函数，则被忽略
- onRejected不是一个函数，则被忽略

**2. 如果onFulfilled是一个函数**（onFulfilled特性）

- 它必须在promise fulfilled后调用，且promise的value为其第一个参数
- 它不能在promise fulfilled前调用
- 其调用次数不可超过一次

**3. 如果onRejected是一个函数**（onRejected特性）

- 它必须在promise rejected后调用，且promise的reason为其第一个参数
- 它不能在promise rejected前调用
- 其调用次数不可超过一次

**4. onFulfilled 和 onRejected 只有在执行环境堆栈仅包含平台代码时才可被调用**（调用时机）

**5. onFulfilled 和 onRejected 必须被作为函数调用（即没有 this 值）**（调用要求）

**6. then 方法可以被同一个 promise 调用多次**（多次调用）

- 当 promise 成功执行时，所有 onFulfilled 需按照其注册顺序依次回调
- 当 promise 被拒绝执行时，所有的 onRejected 需按照其注册顺序依次回调

**7. then 方法必须返回一个 promise 对象**（返回）