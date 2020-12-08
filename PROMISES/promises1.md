##  Promise从入门到自定义
 
```javascript
第1章：准备
检测回调函数是否是异步
在回调函数中输出内容, 在函数后再次输出内容, 进行比对
1.1. 区别实例对象与函数对象
1. 实例对象: new 函数产生的对象, 称为实例对象, 简称为对象
2. 函数对象: 将函数作为对象使用时, 简称为函数对象
//函数对象
        function Person (name,age){
            this.name = name
            this.age = age
        }
        Person.a = 1 //将Person看成一个对象
        const p1 = new Person('老刘',18) //p1是Person的实例对象
        console.log(p1);

总结
  1. 点的左边是对象(可能是实例对象也可能是函数对象)
  2. ()的左边是函数

 
1.2. 二种类型的回调函数
1.2.1. 同步回调
1. 理解: 立即在主线程上执行, 完全执行完了才结束, 不会放入回调队列中
2. 例子: 数组遍历相关的回调函数 / Promise的excutor函数
//演示同步的回调函数
        /* let arr = [1,3,5,7,9]
        arr.forEach((item)=>{
            console.log(item);
        })
        console.log('主线程的代码'); */

1.2.2. 异步回调
异步操作
fs 文件操作
定时器
数据库操作
AJAX

1. 理解: 不会立即执行, 会放入回调队列中将来执行
2. 例子: 定时器回调 / ajax回调 / Promise的成功|失败的回调
// 2. 异步回调函数
setTimeout(() => { // 异步执行的回调函数, setTimeout()在回调函数执行前就结束了, 回调会放入回调队列中在同步代码执行完后才会执行
  console.log('setTimeout callback()')
}, 0)
console.log('setTimeout()之后')

1.3. JS的error处理
1.3.1. 错误的类型
1. Error: 所有错误的父类型
2. ReferenceError: 引用的变量不存在
3. TypeError: 数据类型不正确的错误
4. RangeError: 数据值不在其所允许的范围内
5. SyntaxError: 语法错误
1.3.2. 错误处理
1. 捕获错误: try ... catch
2. 抛出错误: throw error
1.3.3. error对象的结构
1. message属性: 错误相关信息
2. stack属性: 函数调用栈记录信息
 案例:
//演示：ReferenceError: 引用的变量不存在
    /* console.log(a); */

    //演示：TypeError: 数据类型不正确
    /* const demo = ()=>{}
    demo()() */

    //演示：RangeError: 数据值不在其所允许的范围内
    /* const demo = ()=>{demo()}
    demo() */

    //演示：SyntaxError: 语法错误
    /* console.log(1; */

    //如何捕获一个错误
    //try中放可能出现错误的代码，一旦出现错误立即停止try中代码的执行，调用catch，并携带错误信息
    /* try {
        console.log(1);
        console.log(a);
        console.log(2);
    } catch (error) {
        console.log('代码执行出错了,错误的原因是：',error);
    } */

    //如何抛出一个错误
    function demo(){
        const date = Date.now()
        if(date % 2 === 0){
            console.log('偶数，可以正常工作');
        }else{
            throw new Error('奇数，不可以工作！')
        }
    }
    try {
        demo()
    } catch (error) {
        debugger;
        console.log('@',error);
    }



         * 语法固定 try...catch   try 尝试的意思  catch 捕获
         * 1. try catch捕获到错误之后, 后续代码可以继续执行
         * 2. catch 可以将错误信息捕获到. e 是一个对象, 有message和stack两个属性
         * 3. 抛出错误之后, 在后续的 try 里面的代码不会执行
         * 4. try 不能捕获语法错误. 其他三种类型错误可以捕获.
         * 5. 允许使用 throw 手动的抛出错误
         * 6. 抛出任意类型的数据       
        try{
            //抛出一个引用错误
            // console.log(a);
            //这里代码不会执行
            // console.log('abc');
            //手动抛出错误
            // throw new Error('出了点问题');
            //可以抛出任意类型
            throw 'abc';
        }catch(e){
            // console.log('test');
            console.dir(e);
            // alert(e.message);
        }
        console.log(123);

第2章：Promise的理解和使用
2.1. Promise是什么?
2.1.1. 理解
1. 抽象表达:
1) Promise是一门新的技术(ES6规范)
2) Promise是JS中进行异步编程的新解决方案(旧方案是谁?---纯回调)
备注：旧方案是单纯使用回调函数
2. 具体表达:
1) 从语法上来说: Promise是一个构造函数
2) 从功能上来说: promise对象用来封装一个异步操作并可以获取其成功/失败的结果值
2.1.2. promise的状态改变
1. pending变为resolved
2. pending变为rejected
说明: 只有这2种, 且一个promise对象只能改变一次
      无论变为成功还是失败, 都会有一个结果数据
      成功的结果数据一般称为value, 失败的结果数据一般称为reason
2.1.3. promise的基本流程

 1.Promise不是回调，是一个内置的构造函数，是程序员自己new调用的。
 2.new Promise的时候，要传入一个回调函数，它是同步的回调，会立即在主线程上执行，它被称为executor(执行者函数)函数
 3.每一个Promise实例都有3种状态：初始化(pending)、成功(fulfilled)、失败(rejected)
 4.每一个Promise实例在刚被new出来的那一刻，状态都是初始化(pending)
 5.executor函数会接收到2个参数，它们都是函数，分别用形参：resolve、reject接收
                  1.调用resolve函数会：
                            (1).让Promise实例状态变为成功(fulfilled)
                            (2).可以指定成功的value(值)。
                  2.调用reject函数会：
                            (1).让Promise实例状态变为失败(rejected)
                            (2).可以指定失败的reason(原因)。
 案例:
//创建一个Promise实例对象
    const p = new Promise((resolve, reject)=>{
        resolve('ok')
    })
    console.log('@',p); //一般不把Promise实例做控制台输出

2.1.4. promise的基本使用
1) 使用1: 基本编码流程
1. 重要语法
                        new Promise(executor)构造函数
                        Promise.prototype.then方法

2. 基本编码流程
                    1.创建Promise的实例对象(pending状态), 传入executor函数
                    2.在executor中启动异步任务（定时器、ajax请求）
3.根据异步任务的结果，做不同处理：
                                3.1 如果异步任务成功了：
                                                我们调用resolve(value), 让Promise实例对象状态变为成功(fulfilled),同时指定成功的value
                                3.2 如果异步任务失败了：
                                                我们调用reject(reason), 让Promise实例对象状态变为失败(rejected),同时指定失败的reason
4.通过then方法为Promise的实例指定成功、失败的回调函数，来获取成功的value、失败的reason
                                注意：then方法所指定的：成功的回调、失败的回调，都是异步的回调。

        3. 关于状态的注意点：
                    1.三个状态:
                                pending: 未确定的------初始状态
                                fulfilled: 成功的------调用resolve()后的状态
                                rejected: 失败的-------调用reject()后的状态
                    2.两种状态改变
                                pending ==> fulfilled
                                pending ==> rejected
                    3.状态只能改变一次！！
        4.一个promise指定多个成功/失败回调函数, 都会调用吗?


定义一个sendAjax函数，对xhr的get请求进行封装：
                1.该函数接收两个参数：url(请求地址)、data(参数对象)
                2.该函数返回一个Promise实例
                            (1).若ajax请求成功,则Promise实例成功,成功的value是返回的数据。
                            (2).若ajax请求失败,则Promise实例失败,失败的reason是错误提示。


 
2.2. 为什么要用Promise?
2.2.1. 指定回调函数的方式更加灵活
1.       旧的: 必须在启动异步任务前指定
2.       promise: 启动异步任务 => 返回promie对象 => 给promise对象绑定回调函数(甚至可以在异步任务结束后指定/多个)
2.2.2. 支持链式调用, 可以解决回调地狱问题
1.       什么是回调地狱?
回调函数嵌套调用, 外部回调函数异步执行的结果是嵌套的回调执行的条件
2.       回调地狱的缺点? 
不便于阅读
不便于异常处理
3.       一个不是很优秀的解决方案?
promise链式调用
4.       终极解决方案?
async/await
2.3. 如何使用Promise?
2.3.1. API
1.              Promise构造函数: Promise (excutor) {}
(1)      excutor函数:  执行器  (resolve, reject) => {}
(2)      resolve函数: 内部定义成功时我们调用的函数 value => {}
(3)      reject函数: 内部定义失败时我们调用的函数 reason => {}
说明: excutor会在Promise内部立即同步回调,异步操作其中执行

2.       Promise.prototype.then方法: (onFulfilled, onRejected) => {}
(1)      onFulfilled函数: 成功的回调函数  (value) => {}
(2)      onRejected函数: 失败的回调函数 (reason) => {}
说明: 指定用于得到成功value的成功回调和用于得到失败reason的失败回调
返回一个新的promise对象
案例:

 
3.       Promise.prototype.catch方法: (onRejected) => {}
(1)      onRejected函数: 失败的回调函数 (reason) => {}
说明: then()的语法糖, 相当于: then(undefined, onRejected)
 案例:


4.       Promise.resolve方法: (value) => {}
(1)      value: 成功的数据或promise对象
说明: 返回一个成功/失败的promise对象
案例:

 
5.       Promise.reject方法: (reason) => {}
(1)      reason: 失败的原因
说明: 返回一个失败的promise对象
案例:

 
6.       Promise.all方法: (promises) => {}
(1)      promises: 包含n个promise的数组
说明: 返回一个新的promise, 只有所有的promise都成功才成功, 只要有一个失败了就直接失败
 案例:


7.       Promise.race方法: (promises) => {}
(1)      promises: 包含n个promise的数组
说明: 返回一个新的promise, 第一个完成的promise的结果状态就是最终的结果状态
案例:


2.3.2. Promise的几个关键问题
1.              如何改变promise的状态?
(1)      resolve(value): 如果当前是pendding就会变为fulfilled
(2)      reject(reason): 如果当前是pendding就会变为rejected
(3)      抛出异常: 如果当前是pendding就会变为rejected
 案例:


2.              一个promise指定多个成功/失败回调函数, 都会调用吗?
当promise改变为对应状态时都会调用
改变Promise实例的状态和指定回调函数谁先谁后?
                    1.都有可能, 正常情况下是先指定回调再改变状态, 但也可以先改状态再指定回调
                    2.如何先改状态再指定回调?
                                延迟一会再调用then()
                    3.Promise实例什么时候才能得到数据?
                                如果先指定的回调, 那当状态发生改变时, 回调函数就会调用, 得到数据
                                如果先改变的状态, 那当指定回调时, 回调函数就会调用, 得到数据 
案例:


3.       改变promise状态和指定回调函数谁先谁后?
(1)      都有可能, 正常情况下是先指定回调再改变状态, 但也可以先改状态再指定回调
(2)      如何先改状态再指定回调?
①      在执行器中直接调用resolve()/reject()
②      延迟更长时间才调用then()
(3)      什么时候才能得到数据?
①      如果先指定的回调, 那当状态发生改变时, 回调函数就会调用, 得到数据
②      如果先改变的状态, 那当指定回调时, 回调函数就会调用, 得到数据
 
4.              promise.then()返回的新promise的结果状态由什么决定?
(1)      简单表达: 由then()指定的回调函数执行的结果决定
(2)      详细表达:
①      如果抛出异常, 新promise变为rejected, reason为抛出的异常
②      如果返回的是非promise的任意值, 新promise变为fulfilled, value为返回的值
③      如果返回的是另一个新promise, 此promise的结果就会成为新promise的结果
Promise实例.then()返回的是一个【新的Promise实例】，它的值和状态由什么决定?
                    1.简单表达: 由then()所指定的回调函数执行的结果决定
                    2.详细表达:
                            (1)如果then所指定的回调返回的是非Promise值a:
                                            那么【新Promise实例】状态为：成功(fulfilled), 成功的value为a
                            (2)如果then所指定的回调返回的是一个Promise实例p:
                                            那么【新Promise实例】的状态、值，都与p一致
                            (3)如果then所指定的回调抛出异常:
                                            那么【新Promise实例】状态为rejected, reason为抛出的那个异常
案例:


5.       promise如何串连多个操作任务?
(1)      promise的then()返回一个新的promise, 可以开成then()的链式调用
(2)      通过then的链式调用串连多个同步/异步任务
案例:


6.       promise异常传透?
(1)      当使用promise的then链式调用时, 可以在最后指定失败的回调,
(2)      前面任何操作出了异常, 都会传到最后失败的回调中处理
promise错误穿透：
                    (1)当使用promise的then链式调用时, 可以在最后用catch指定一个失败的回调,
                    (2)前面任何操作出了错误, 都会传到最后失败的回调中处理了
            备注：如果不存在then的链式调用，就不需要考虑then的错误穿透。
案例:
  <script>
        //另一个例子演示错误的穿透
        const p = new Promise((resolve,reject)=>{
            setTimeout(()=>{
                reject(-100)
            },1000)
        })
        p.then(
            value => {console.log('成功了1',value);return 'b'},
            reason => {throw reason}//底层帮我们补上的这个失败的回调
        )
        .then(
            value => {console.log('成功了2',value);return Promise.reject(-108)},
            reason => {throw reason}//底层帮我们补上的这个失败的回调
        )
        .catch(
            reason => {throw reason}
        )

        function sendAjax(url,data){
            return new Promise((resolve,reject)=>{
                //实例xhr
                const xhr = new XMLHttpRequest()
                //绑定监听
                xhr.onreadystatechange = ()=>{
                    if(xhr.readyState === 4){
                        if(xhr.status >= 200 && xhr.status < 300) resolve(xhr.response);
                        else reject(`请求出了点问题`);
                    }
                }
                //整理参数
                let str = ''
                for (let key in data){
                    str += `${key}=${data[key]}&`
                }
                str = str.slice(0,-1)
                xhr.open('GET',url+'?'+str)
                xhr.responseType = 'json'
                xhr.send()
            })
        }
        //利用错误的穿透避免多次指定失败的回调
        sendAjax('https://api.apiopen.top/getJoke2',{page:1})
        .then(
            value => {
                console.log('第1次请求成功了',value);
                //发送第2次请求
                return sendAjax('https://api.apiopen.top/getJoke',{page:1})
            },
            // reason => {console.log('第1次请求失败了',reason);return new Promise(()=>{})}
        )
        .then(
            value => {
                console.log('第2次请求成功了',value);
                //发送第3次请求
                return sendAjax('https://api.apiopen.top/getJoke',{page:1},3)
            },
            // reason => {console.log('第2次请求失败了',reason);return new Promise(()=>{})}
        )
        .then(
            value => {console.log('第3次请求成功了',value);},
            // reason => {console.log('第3次请求失败了',reason);return new Promise(()=>{})}
        )
        .catch(
            reason => {console.log(reason);}
        )

```javascript
7.       中断promise链?
(1)      当使用promise的then链式调用时, 在中间中断, 不再调用后面的回调函数
(2)      办法: 在回调函数中返回一个pendding状态的promise对象
案例:


优势
1. 指定回调函数的方式更加灵活: 
                    旧的: 必须在启动异步任务前指定
                    promise: 启动异步任务 => 返回promie对象 => 给promise对象绑定回调函数(甚至可以在异步任务结束后指定)

2. 支持链式调用, 可以解决回调地狱问题
                    (1)什么是回调地狱：
                                    回调函数嵌套调用, 外部回调函数异步执行的结果是嵌套的回调函数执行的条件
                    (2)回调地狱的弊病：
                                    代码不便于阅读、不便于异常的处理
                    (3)一个不是很优秀的解决方案：
                                    then的链式调用
                    (4)终极解决方案：
                                    async/await（底层实际上依然使用then的链式调用）

第4章：async与await
4.1. mdn文档
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/await
4.2. async 函数
1.       函数的返回值为promise对象
2.       promise对象的结果由async函数执行的返回值决定
4.3. await 表达式
1.       await右侧的表达式一般为promise对象, 但也可以是其它的值
2.       如果表达式是promise对象, await返回的是promise成功的值
3.       如果表达式是其它值, 直接将此值作为await的返回值
4.4. 注意
1.       await必须写在async函数中, 但async函数中可以没有await
2.       如果await的promise失败了, 就会抛出异常, 需要通过try...catch捕获处理

案例:


await的原理
若我们使用async配合await这种写法：
         1.表面上不出现任何的回调函数
         2.但实际上底层把我们写的代码进行了加工，把回调函数“还原”回来了。
         3.最终运行的代码是依然有回调的，只是程序员没有看见。
案例:


第5章：JS异步之宏队列与微队列
5.1. 原理图

5.2. 说明
1.       JS中用来存储待执行回调函数的队列包含2个不同特定的列队
2.       宏列队: 用来保存待执行的宏任务(回调), 比如: 定时器回调/DOM事件回调/ajax回调
3.       微列队: 用来保存待执行的微任务(回调), 比如: promise的回调/MutationObserver的回调
4.       JS执行时会区别这2个队列
(1)     JS引擎首先必须先执行所有的初始化同步任务代码
(2)     每次准备取出第一个宏任务执行前, 都要将所有的微任务一个一个取出来执行

  宏队列:[宏任务1，宏任务2.....]
      微队列:[微任务1，微任务2.....]
      规则：每次要执行宏队列里的一个任务之前，先看微队列里是否有待执行的微任务
            1.如果有，先执行微任务
            2.如果没有，按照宏队列里任务的顺序，依次执行
微队列比宏队列权重大 目前只有promise是微任务

```

