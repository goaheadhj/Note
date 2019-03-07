# DAV介绍

## 培训目标

1. 介绍DVA是什么？ 

2. DVA解决什么问题？怎么解决的？ 

3. 一个DVA项目的组成，及各部分是干什么的？ 

4. 使用DVA开发功能的工作流是什么样的？ 

5. 我们还增加了哪些功能去完善项目的开发？ 

### DVA是什么？ 

DVA是一个打包了redux，redux-saga的数据流方案。 为了方便开发，同时内置了fetch，react-router。   

#### 特性

* 易学易用，仅有 6 个 api，对 redux 用户尤其友好 

* elm 概念，通过 reducers, effects 和 subscriptions 组织 model 

* 插件机制，比如 dva-loading 可以自动处理 loading 状态，不用一遍遍地写 showLoading 和 hideLoading 

* 支持 HMR，基于 babel-plugin-dva-hmr 实现 components、routes 和 models 的 HMR 

#### DVA解决什么问题？

##### 问题 

将data的的状态变化过程，从view中剥离出来。使表现和状态管理分离，各层的功能更加单一。   

##### 解决方案 

使用redux管理data，使用saga调度action。   

#### DVA项目的基本组成

##### 项目的目录结构 

```
-- src   
	-- assets // 代码中，通过CSS或者JS引入的静态文件。 
	-- components // 和redux无关的通用组件。react组件 
	-- models // 状态管理模块。dva的核心功能部分。javascript对象 
	-- routes // 路由对应的入口组件。react组件 
	-- services // api请求的模块。es模块 
	-- utils // 通用的工具方法。es模块 
```

**services** 

将所有的api请求单独抽取出来作为一层，全部放到services中管理起来。可以参照rest风格，按不同的资源类型来组织api请求，将同一类的资源操作放在一个文件里。 

因为所有的api请求都是异步请求，所以可以将所有的api请求方法都写成async函数，使用同步的方式调用。 

**routes** 

routes中的文件都是一个react组件，每一个组件对应了一个url路由路径。每个组件作为对应路由的入口，同时作为redux中的container组件，和redux进行关联， 为这个页面下的字组件提供数据的入口。 

**models** 

每个model都是一个javascript对象，用来管理一个路由页面下的页面数据和状态。 

这个对象有这样几个属性 

* namespace - 表示在全局state上的key 

*  state - 表示这个model所管理数据的初始值 

*  reducers - 等同于 redux 里的 reducer，接收 action，同步更新 state 

* effects - 处理异步action的方法。通常将一个完整的事务操作封装成一个effect。每个effect是一个generator函数，当通过effect的名字调用effect时，dva会自动执行这effect 

##### 工作原理

![PPrerEAKbIoDZYr.png](DAV介绍.assets/6572498A-E8D0-5741-8542-3EF4B5C3B7CA.png)

### DVA开发的工作流

1. 在routes中创建路由组件。 

2. 在components中创建UI组件。 

3. 在models中创建model。 

4. 在route中关联model。 

5. 在component中，通过dispatch，发送action给model中的effect和reducer。   

### 在开发中继续完善DVA

#### 完善创建action的过程。 

在dva中，发送action到model中的方式，是通过将model中的effect或者reducer的方法名作为字符串传递给dispatch。这种通过字符串控制代码逻辑的方式非常容易出错，并且不容排错，所以这通过一个工具方法createAction来创建model中提供的action，然后将这些action作为model提供的api，暴露出来。 

#### 增加请求数据时的动态类型检查 

在请求后台接口时，事先定义好约定的数据模型。在获得服务端返回数据时，对返回的数据做类型检查，如果和事先约定的数据类型不一致，提前将错误暴露出来。 