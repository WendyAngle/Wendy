---
title: koa便签
categories: 源码
tag: [koa]
date: 2017-07-29 13:00:00
comments: true
---

### koa调用方式
http
```
app.listen(3000);
//等同于
http.createServer(app.callback()).listen(3000);
```
https
```
https.createServer(credentials, app.callback()).listen(3000)
```

### 一些相同引用或者属性
**koa实例**    
app.toJSON == app.inspect

**request**    
ctx.request.header == ctx.request.headers

**response**    
ctx.response.header == ctx.response.headers


### koa实例 use方法
use 返回的是this,也就是实例本身，那么就是可以连续use    
app.use(middleware1).use(middleware2)  

### app.use里面的ctx
* ctx.req保存了http里面原始的request对象    
* ctx.res保存了http里面原始的response对象   
* ctx.request.reponse保持对ctx.reponse引用, 也就是可以玩躲猫猫， ctx.request.reponse.request
* ctx.response.request保持对ctx.request引用
* ctx 代理了所有request和response的方法，但是使用的时候，自己注意哪些属性能使用
* ctx.originalUrl保存req.url,也就是原生request.url





