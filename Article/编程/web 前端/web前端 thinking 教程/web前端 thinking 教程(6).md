# web前端 thinking 教程(6)
## 教学提纲
- 作业问题讲解
- restful api
- 架构模式讲解
    - MV*
    - react

## 作业问题讲解
将一份完整作业讲解。

## restful api
> 是一种前后端分离开发方式，后端提供 restful api，前端通过调用 ajax 调用 restful api，来对后端数据进行增删改查

阅读： 理解RESTful架构 http://www.ruanyifeng.com/blog/2011/09/restful.html

核心摘要：
> 五、状态转化（State Transfer）
访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。
互联网通信协议HTTP协议，是一个无状态协议。这意味着，所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。
客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。

> 六、综述
综合上面的解释，我们总结一下什么是RESTful架构：
1. 每一个URI代表一种资源；
2. 客户端和服务器之间，传递这种资源的某种表现层；
3. 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。


## 架构模式讲解
> 有了 ajax 后，前端就可以自由的操作（增删改查） model，restful api 是典型代表，也就是说前端也有了 M 层

界面之下：还原真实的MV*模式: http://www.html-js.com/article/3198

（这次的阅读有一定难度，请结合后面的内容解读来阅读）

如果对 MVC 了解吃力 MVP 方面的知识可以略读。重点在 MVC 和 MVVM，并且在这里，架构模式首先是用在整个 web 应用（前端+后端,前端只是一个 view，css 是主力，然后 js 只是动画、小脚本。。。），到了 ajax 应用崛起之后，前端方面开始有独立的整个架构模式，这就是分形架构模式（远看 web 应用整个是一个 MVC。近看，前端有一个 MV*，后端又有一个 MVC），react 里，js 占比重60%+，甚至可以服务器端也全是 js，安卓 ios 用 react native，全端都是 js。。。。

核心内容解读：
- MV* 都是为了解决图形界面应用程序复杂性管理问题而产生的应用架构模式
    - 复杂性管理问题: 有维护数据、响应事件、维护视图、数据和视图的绑定(数据更改后，视图应该也要修改)
    - 应用架构模式：是一种分离职责的模式，使用不同的模块来分别维护应用的不同部分，让每个模块各司其职。
- 职责详解（以 MVC 为例）
    - Model 提供数据操作的接口，执行相应的业务逻辑，比如数据的增删改查的接口，都应该由 Model 维护，复杂的业务应该是其他模块去调用这些接口来实现，而不是写在 Model 里。Model 仅仅提供纯粹的数据操作接口，在 MVC 模式中，Model 还会实现观察者模式（和 js 的 addEventListener 原理一样一样的，dom 里是对事件的监听，这里是 view 对 model 的数据的改变的监听，然后当数据改变后，view 根据自己的需求去显示最新的样子）。
    - View 实现整个页面的样子，分两部分，第一部分是 构造 View 时，通过 Model 层的数据来构造，比如文章数据在 model 里面，view 通过查，得到数据再展示出来。第二部分是，当 model 被修改后，view 应该更新自己，这个的实现，在 MVC 中，View 会根据自己的需求（比如，如果 view 是评论 view，那么它就会去订阅 评论数据的更改 的事件）去订阅Model变更的消息
    - Controller 操作 Model，当响应事件（用户点赞、评论等）的时候，C 会去调用 Model 提供的接口，来执行业务逻辑对数据进行处理。但不会直接操作View，可以说它是对View无知的。
- MVC 的优缺点
    - 优点：把业务逻辑和展示逻辑分离，模块化程度高。当 model 更改后，观察者模式可以做到多视图同时更新（这里的体现是，页面内有多处视图，比如）。
    - 缺点:
        - Controller 测试困难。因为视图同步操作是由 View 自己执行，而 View 只能在有UI的环境下运行。在没有UI环境下对 Controller 进行单元测试的时候，应用逻辑正确性是无法验证的：Model更新的时候，无法对View的更新操作进行断言。
        - View 无法组件化。View 是强依赖特定的 Model 的
- web 开发中的 MVC
    - 实际上 C 层是路由层（路由规则 + 响应处理），http 请求发过来后，后端根据路由规则交给 C 去进行响应处理。
- MVVM(= M + V + VM)
    - M 层和 V 层，通过 VM 层双向绑定，达到了双向数据绑定(当 model 中的数据更改后，view 会自动更新)。
    - 缺点:
        - 对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高
        - 数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的(react 大法好？不过 react 代码量确实是会比 view 多不少)
