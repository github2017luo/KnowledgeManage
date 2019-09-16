# 你问我答
<details>
    <summary>React</summary>
    <pre>
    <p>
        React 性能优化
    </p>
    <ol>
        <li>shouldcomponentupdate</li>
        <li>purecomponent 纯组件 prop state没改变 不会触发render</li>
        <li>key</li>
        <li>避免render内部新建变量和bind函数</li>
        <li>Immutable</li>
    </ol>
    </pre>
</details>

## React Fiber
    1.从reactDOM.render()变成了ReactDOMFiber.render()。
    我们使用了ReactFiber去渲染整个页面，ReactFiber会将整个更新任务分成若干个小的更新任务，
    然后设置一些任务默认的优先级。每执行完一个小任务之后，会释放主线程。
## React Hooks
    1.难以重用和共享组件 逻辑复杂的组件难以开发与维护
    3告别了繁杂的this和合并了难以记忆的生命周期。
    4.支持包装自己的Hooks(自定义Hooks)，是基于纯命令式的api。
    5.更好的完成状态之间的共享，解决原来class组件内部封装的问题，也解决了高阶组件和函数组件的嵌套过深。一个组件一个自己的state，一个组件内可以公用
    6.对于类组件中使用 shouldComponentUpdate 进行优化的地方，
    可以使用 React.memo 包裹整个组件，对 props 进行浅比较来判断。
    合理地规范传入的props，从根本上减少不必要地渲染
## 函数式组件的特性
    1.无状态
    2.无生命周期方法
    3.无this
    所以，在之前，函数式组件更多地被用来当作展示性组件（presentational component）：只是单纯地接受props并且展示内容。
    1.其实换过来想，函数式组件因为没有生命周期，也就没有了shouldComponentUpdate，
    那么如果编写不当，可能反而会导致过多无用地重绘，因为它没法判断是否需要重新绘制组件，组件本身是个函数就都会执行。
## Vue nextTick
    1.在下次 DOM 更新循环结束之后执行延迟回调 更新异步
## CSS BFC
    1.盒子并且至少满足下列条件中的任何一个：
        float的值不为none
        position的值不为static或者relative
        display的值为 table-cell, table-caption, inline-block, flex, 或者 inline-flex中的其中一个
        overflow的值不为visible
    2.BFC来防止外边距折叠
    3.使用BFC来包含浮动
    4.一个容器里有浮动元素。由于这个原因，容器元素没有高度，
    它的浮动孩子将会脱离页面的常规流。我们通常使用清除浮动来解决这个问题，
    最受欢迎的方法是使用一个clearfix的伪类元素。但我们同样可以通过定义一个BFC来达到这个目的著作权归作者所有。
## CSS 清除浮动
    1.浮动元素脱离了文档流，并不占据文档流的位置，自然父元素也就不能被撑开，所以没了高度。
    2.clear
    3.BFC
## JS Event loop
    1.虽然js是单线程，但是完全可以模拟多线程，靠的就是Event Loop。
    2.异步任务 分为 微任务(microtask) 和 宏任务(task)，执行的顺序是 执行栈中的代码 => 微任务 => 宏任务
    3.执行栈

        执行栈中的代码永远最先执行
        微任务(microtask):
            当执行栈中的代码执行完毕，会在执行宏任务队列之前先看看微任务队列中有没有任务，如果有会先将微任务队列中的任务清空才会去执行宏任务队列
        宏任务(task): setTimeout setInterval setImmediate(IE专用)
            等待执行栈和微任务队列都执行完毕才会执行，并且在执行完每一个宏任务之后，会去看看微任务队列有没有新添加的任务，如果有，会先将微任务队列中的任务清空，才会继续执行下一个宏任务
## JS 节流、防抖
    1.函数防抖(debounce):在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。input 更改请求
    2.函数节流(throttle)：规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。
## 预加载
    1.preload: 让浏览器提前加载指定资源(加载后并不执行)，在需要执行的时候再执行
## 缓存
    1.强缓存：Cache-Control和Expire两个字段控制 
    Cache-Control的值为“public, max-age=xxx”，表示在xxx秒内再次访问该资源，均使用本地的缓存，不再向服务器发起请求
    2.协商缓存： 协商缓存最大的问题就是每次都要向服务器验证一下缓存的有效性
    3.webpack hash
## 从输入URL到页面加载的过程
    1.从浏览器接收url到开启网络请求线程
        1.浏览器是多进程的，有一个主控进程，以及每一个tab页面都会新开一个进程
        2.每一个tab页面可以看作是浏览器内核进程，然后这个进程是多线程的
        3.解析URL
        4.开辟单独的线程进行网络请求
        5.输入的是域名，需要进行dns解析成IP
        6.http的本质就是tcp/ip请求
        7.tcp将http长报文划分为短报文，通过三次握手与服务端建立连接
        8.需要进行四次挥手断开连接
    2.解析页面流程
        1. 解析HTML，构建DOM树
        2. 解析CSS，生成CSS规则树
        3. 合并DOM树和CSS规则，生成render树
        4. 布局render树（Layout/reflow），负责各元素尺寸、位置的计算
        5. 绘制render树（paint），绘制页面像素信息
        6. 浏览器会将各层的信息发送给GPU，GPU会将各层合成（composite），显示在屏幕上
## async 普通函数
    1.async 函数的实现，就是将 Generator 函数和自动执行器
    2.当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。
## webpack 优化
    1.关闭sourcemap
    2.将不变文件cdn打包 提取公共库包 减少打包文件 dll
    3.调用多进程打包 happypack
    4.压缩代码
    5.雪碧图
    6.loader include exclude
## Mobx
```
    import { observable, autorun } from 'mobx';

    const counter = observable(0);
    autorun(() => {
      console.log('autorun', counter.get());
    });

    counter.set(1);
    // 0
    // 1
    // set后会重新触发autorun
    // autorun 先生成一个Derivation类 执行传入函数 计算出observing(通过get方法)
    // set之后会从observing中取他的Derivation 触发重新执行
```
```
    import { observable, autorun } from 'mobx';
    
    const counter = observable(0);
    const foo = observable(0);
    const bar = observable(0);
    autorun(() => {
      if (counter.get() === 0) {
        console.log('foo', foo.get());
      } else {
        console.log('bar', bar.get());
      }
    });
    
    bar.set(10);    // 不触发 autorun
    counter.set(1); // 触发 autorun
    foo.set(100);   // 不触发 autorun
    bar.set(100);   // 触发 autorun
    // foo 0
    // bar 10
    // bar 100
    // 通过get知道 autorun先是依赖counter和foo
    // counter.set后依赖counter bar
```
    
    1.Object.defineProperty，proxy 拦截 getter 和 setter
    2.Mobx 最关键的函数在于 autoRun
    3.Observable 。用来包装一个属性为 被观察者
    autorun 。用来包装一个方法为 观察者
    2.对比Redux

    Redux将数据保存在单一store中，Mobx将数据保存在分散的多个store中
    Redux需要手动处理变化后的操作，Mobx使用observable保存数据，数据变化后自动处理响应的操作
    Redux使用不可变状态，不能直接去修改它，而是应该使用纯函数返回一个新的状态；Mobx中的状态是可以直接修改的
## promise.all
    1.这个返回的 promise 之后会在所有的 promise 都完成或有一个 promise 失败时异步地变为完成或失败
## CSS 居中
    1.flex 2.grid 3.margin: 0 auto; text-align: center 4.table 5.绝对布局 相对布局
## call apply bind
    1.只参数形式不一样 调用即运行 
    2/bind会返回一个新的函数 都用于改变运行时的上下文
## requestAnimationFrame(code)
    1.一般用于动画，与 setTimeout 方法类似，区别是 setTimeout 是用户指定的，
    而 requestAnimationFrame 是浏览器刷新频率决定的，一般遵循 W3C 标准，它在浏览器每次刷新页面之前执行。
## 前端安全
    1.XSS：是通过在你的输入文本当中或者这HTML标签当中插入js脚本进行攻击 解决：插入内容用innerText 对一些特殊标签进行转义
    2.CSRF: b站发送的请求带着a站的cookie信息； b站发送请求不经过a站的前端；http请求头中的referer为b站 
    iframe内嵌某一个网站 cookie 解决：same-site属性 referer来源
    3.内嵌iframe： Javascript禁止内嵌：当网页没有被使用iframe内嵌时，top和window是相等的；当网页被内嵌时，top和window是不相等的
## React diff
    1.老diff算法O(n3) react diff O(n)
    2.Web UI中dom节点跨层级移动操作很少 react对virtual dom进行分层比较 如果一个节点不存在 则改节点和子节点完全删除 发现多了节点 会直接创建节点
    3.组件比较 如果是同一类型的按照原策略比较virtual dom树 如果不是 就直接替换该节点以及所有子节点
    4.同一层级的节点 diff算法有插入 移动 删除的操作 如果同一位置节点不同 就直接删除然后然后创建插入
    所以react推荐添加唯一的key进行区分
## Redux原理
    1.TODO



