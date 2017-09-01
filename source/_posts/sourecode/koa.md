---
title: koa源码分析
categories: 源码
tag: [koa]
date: 2017-07-29 18:00:00
comments: true
---
在阅读文章之前，最好对koa(koa 2)有个基本的了解，最简单的方式从如下链接了解    
[Koa - next generation web framework for node.js](http://koajs.com/)    
[koajs/koa: Expressive middleware for node.js using ES2017 async functions](https://github.com/koajs/koa)    
[Koa](https://www.npmjs.com/package/koa)


## koa 源码结构
* application.js    
     入口文件，导出  class Application            
      `const Koa = require('koa');`,代码里面的Koa就是Application     
* context.js     
    可以理解为中间件里面的ctx,比如如下的ctx
```javascript
app.use(async ctx => {
  ctx.body = 'Hello World';
});    
```
* request     
对原生request的一些属性封装 
* response   
对原生response的一些属性封装 


## 源码分析
### request.js
都是返回一个对象，分别对原生request进行了封装，    
注意的是 this.req, 整个文件中并没有req的申明,其就是原生http中request对象的引用    
this.ctx 是对ctx的引用  
这个等到context.js和application.js再解释，
```
module.exports = {
  /**
   * Return request header.
   *
   * @return {Object}
   * @api public
   */

  get header() {
    return this.req.headers;
  },
  ......
```    
    
### response.js
都是返回一个对象，分别对原生response进行了封装，    
注意的是 this.res, 整个文件中并没有res的申明,其就是原生http中response对象的引用      
this.ctx是对ctx的引用，
这个等到context.js和application.js再解释，
```javascript
module.exports = {
  /**
   * Return the request socket.
   *
   * @return {Connection}
   * @api public
   */

  get socket() {
    return this.ctx.req.socket;
  },
   /**
   * Return response header.
   *
   * @return {Object}
   * @api public
   */
  get header() {
    const { res } = this;
    return typeof res.getHeaders === 'function'
      ? res.getHeaders()
      : res._headers || {};  // Node < 7.7
  },
  ......
```

### context.js
本身不复杂，申请一个对象，然后调用delegate代理方法和属性    
妙就妙在这个 delegate，个人觉得是koa精髓之一
```javascript
const createError = require('http-errors');
const httpAssert = require('http-assert');
const delegate = require('delegates');
const statuses = require('statuses');

const proto = module.exports = {
    ......
}

delegate(proto, 'response')
  .method('attachment')
  .method('redirect')
  .method('remove')
  .method('vary')
  .method('set')
  .method('append')
  .method('flushHeaders')
  .access('status')
  .access('message')
  .access('body')
  .access('length')
  .access('type')
  .access('lastModified')
  .access('etag')
  .getter('headerSent')
  .getter('writable');
  ......
```
我们来看一段delegates的源码, 源码在github的地址为 [node delegates](https://github.com/tj/node-delegates)
```javascript
/**
 * Initialize a delegator.
 *
 * @param {Object} proto
 * @param {String} target
 * @api public
 */

function Delegator(proto, target) {
  if (!(this instanceof Delegator)) return new Delegator(proto, target);
  this.proto = proto;
  this.target = target;
  this.methods = [];
  this.getters = [];
  this.setters = [];
  this.fluents = [];
}

Delegator.prototype.method = function(name){
  var proto = this.proto;
  var target = this.target;
  this.methods.push(name);
  //添加方法，
  proto[name] = function(){
    //实际执行的时候，是 proto target属性上的name方法
    return this[target][name].apply(this[target], arguments);
  };

  return this;
};
```
简单理解就是在proto上添加了一个方法，实际执行的时候是执行proto对象target属性上的方法，方法执行时的上下文是target属性    
那么回归我们的源码，    
**context.js 那里面一堆的method,access,getter就是把request和response的方法在context在开了一个窗口，最终还是执行request和response的方法或者getter，setter
最终的结果就是ctx有所有response和request的方法和getter,setter。主要是方便使用了。**     
额外提一下   
delegate.access 表示可以读写   
delegate.setter 表示可以写  
delegate.getter 表示可以读 



我们再来看一段 application.js的代码
```javascript
......
const context = require('./context');
......

module.exports = class Application extends Emitter {
  /**
   * Initialize a new `Application`.
   *
   * @api public
   */

  constructor() {
    super();

    this.proxy = false;
    this.middleware = [];
    this.subdomainOffset = 2;
    this.env = process.env.NODE_ENV || 'development';
    // ##1 
    this.context = Object.create(context);
    this.request = Object.create(request);
    this.response = Object.create(response);
  }

  createContext(req, res) {
    // ##2
    const context = Object.create(this.context);
    const request = context.request = Object.create(this.request);
    const response = context.response = Object.create(this.response);
    context.app = request.app = response.app = this;
    context.req = request.req = response.req = req;
    context.res = request.res = response.res = res;
    request.ctx = response.ctx = context;
    request.response = response;
    response.request = request;
    context.originalUrl = request.originalUrl = req.url;
    context.cookies = new Cookies(req, res, {
      keys: this.keys,
      secure: request.secure
    });
    request.ip = request.ips[0] || req.socket.remoteAddress || '';
    context.accept = request.accept = accepts(req);
    context.state = {};
    return context;
  }
```
注意`##1`，`##2`标记的地方，重点看 ##2，先看这3行
```javascript
    const context = Object.create(this.context);
    const request = context.request = Object.create(this.request);
    const response = context.response = Object.create(this.response);
```
先创建context,实际上这个时候，已经有request和repsonse的方法或者getter，setter了，但是光有这个不行啊，因为最终调用时 request和response的方法，第二三句代码就是添加request和response属性。

解析来看看这几句代码，就能解惑上面 response和request的疑问了    
```javascript
    context.app = request.app = response.app = this;
    context.req = request.req = response.req = req;
    context.res = request.res = response.res = res;
    request.ctx = response.ctx = context;
    request.response = response;
    response.request = request;
```
* ctx,request,response都有对app的引用，也是就是Koa()实例             
* ctx,request,response都有对原生request对象的引用       
* ctx,request,response都有对原生response对象的引用      
* request,response都有对content(ctx)的应用       
* request有对response的引用    
* response有对request的引用    
还是用一张图来说明吧    
![app,ctx,request,response,原生requst,response关系图](/blog/img/koa.jpg)

### application.js
删掉一些不重要的代码,仔细看注释
```
module.exports = class Application extends Emitter {
  /**
   * Initialize a new `Application`.
   *
   * @api public
   */

  constructor() {
    super();

    this.proxy = false;
    this.middleware = [];
    this.subdomainOffset = 2;
    this.env = process.env.NODE_ENV || 'development';
    this.context = Object.create(context);
    this.request = Object.create(request);
    this.response = Object.create(response);
  }

  /**
   * Shorthand for:
   *
   *    http.createServer(app.callback()).listen(...)
   *
   * @param {Mixed} ...
   * @return {Server}
   * @api public
   */

  listen() {
    debug('listen');
    const server = http.createServer(this.callback());
    return server.listen.apply(server, arguments);
  } 

  /**
   * Use the given middleware `fn`.
   *
   * Old-style middleware will be converted.
   *
   * @param {Function} fn
   * @return {Application} self
   * @api public
   */

  use(fn) {
    if (typeof fn !== 'function') throw new TypeError('middleware must be a function!');
    if (isGeneratorFunction(fn)) {
      deprecate('Support for generators will be removed in v3. ' +
                'See the documentation for examples of how to convert old middleware ' +
                'https://github.com/koajs/koa/blob/master/docs/migration.md');
      fn = convert(fn);
    }
    debug('use %s', fn._name || fn.name || '-');
    this.middleware.push(fn);
    return this;
  }

  /**
   * Return a request handler callback
   * for node's native http server.
   *
   * @return {Function}
   * @api public
   */

  callback() {
    const fn = compose(this.middleware); //编译中间件

    if (!this.listeners('error').length) this.on('error', this.onerror);//没有定义错误捕获，就采用默认的

    const handleRequest = (req, res) => {
      res.statusCode = 404;
      const ctx = this.createContext(req, res);
      const onerror = err => ctx.onerror(err);
      const handleResponse = () => respond(ctx);
      onFinished(res, onerror);
      return fn(ctx).then(handleResponse).catch(onerror);
    };

    return handleRequest;
  }

  /**
   * Initialize a new context.
   *
   * @api private
   */

  createContext(req, res) {
    const context = Object.create(this.context);
    const request = context.request = Object.create(this.request);
    const response = context.response = Object.create(this.response);
    context.app = request.app = response.app = this;
    context.req = request.req = response.req = req;
    context.res = request.res = response.res = res;
    request.ctx = response.ctx = context;
    request.response = response;
    response.request = request;
    context.originalUrl = request.originalUrl = req.url;
    context.cookies = new Cookies(req, res, {
      keys: this.keys,
      secure: request.secure
    });
    request.ip = request.ips[0] || req.socket.remoteAddress || '';
    context.accept = request.accept = accepts(req);
    context.state = {};
    return context;
  }

  /**
   * Default error handler.
   *
   * @param {Error} err
   * @api private
   */

  onerror(err) {
    assert(err instanceof Error, `non-error thrown: ${err}`);

    if (404 == err.status || err.expose) return;
    if (this.silent) return;

    const msg = err.stack || err.toString();
    console.error();
    console.error(msg.replace(/^/gm, '  '));
    console.error();
  }
};

/**
 * Response helper.
 */

function respond(ctx) {
  // allow bypassing koa
  if (false === ctx.respond) return;

  const res = ctx.res;
  if (!ctx.writable) return;

  let body = ctx.body;
  const code = ctx.status;

  // ignore body
  if (statuses.empty[code]) {
    // strip headers
    ctx.body = null;
    return res.end();
  }

  if ('HEAD' == ctx.method) {
    if (!res.headersSent && isJSON(body)) {
      ctx.length = Buffer.byteLength(JSON.stringify(body));
    }
    return res.end();
  }

  // status body
  if (null == body) {
    body = ctx.message || String(code);
    if (!res.headersSent) {
      ctx.type = 'text';
      ctx.length = Buffer.byteLength(body);
    }
    return res.end(body);
  }

  // responses
  if (Buffer.isBuffer(body)) return res.end(body);
  if ('string' == typeof body) return res.end(body);
  if (body instanceof Stream) return body.pipe(res);

  // body: json
  body = JSON.stringify(body);
  if (!res.headersSent) {
    ctx.length = Buffer.byteLength(body);
  }
  res.end(body);
}
```
#### listen
是http.createServer(app.callback()).listen(...)的一种便捷调用形式    
要是网站使用https协议，那么就得
https.createServer(credentials,capp.callback()).listen(...)
#### use
存中间件的信息，当然中间对采用generator实现的中间件进行了提示    
关于中间件里面的那个next，可以参看 [koa2 中间件里面的next到底是什么 ](http://www.cnblogs.com/cloud-/p/7239819.html)
#### callback
  核心之核心,主要工作    
  * 编译中间件，
  * 错误函数定义
  * 执行中间件，
  * 对请求作出相应  
  * 如果有错误，错误处理    
#### createContext
构建中间件里面的ctx
#### onerror
默认错误捕捉函数
#### respond
实际作出相应的方法
