---
title: koa第三方中间件
categories: 源码
tag: [koa]
date: 2017-07-29 13:00:00
comments: true
---
kao一共引用了24个第三方包，其中我觉得比较喜欢的是 [only](https://www.npmjs.com/package/only)，[delegates](https://www.npmjs.com/package/delegates)      
我全部列在下面，一方面是方便自己查阅和参考，另一方面方便他人解读koa的时候，方便查看这些package是干嘛的   

## koa 引用的第三方package
* [accepts](https://www.npmjs.com/package/accepts)    
Higher-level content negotiation
* [content-type](https://www.npmjs.com/package/content-type)    
Create and parse HTTP Content-Type header according to RFC 7231
* [content-disposition](https://www.npmjs.com/package/content-disposition)   
Create and parse HTTP Content-Disposition header
* [cookies](https://www.npmjs.com/package/cookies)    
Cookies is a node.js module for getting and setting HTTP(S) cookies. Cookies can be signed to prevent tampering, using Keygrip. It can be used with the built-in node.js HTTP library, or as Connect/Express middleware.    
* [debug](https://www.npmjs.com/package/debug)    
A tiny JavaScript debugging utility modelled after Node.js core's debugging technique. Works in Node.js and web browsers.    
* [delegates](https://www.npmjs.com/package/delegates)    
Node method and accessor delegation utilty
* [depd](https://www.npmjs.com/package/depd)    
Deprecate all the things
* [destroy](https://www.npmjs.com/package/destroy)    
Destroy a stream.
This module is meant to ensure a stream gets destroyed, handling different APIs and Node.js bugs.
* [error-inject](https://www.npmjs.com/package/error-inject)     
inject an error listener into a stream 
* [escape-html](https://www.npmjs.com/package/escape-html)    
Escape string for use in HTML
* [fresh](https://www.npmjs.com/package/fresh)      
HTTP response freshness testing
* [http-assert](https://www.npmjs.com/package/http-assert)    
Assert with status codes. Like ctx.throw() in Koa, but with a guard
* [http-errors](https://www.npmjs.com/package/http-errors)    
Create HTTP errors for Express, Koa, Connect, etc. with ease
* [is-generator-function](https://www.npmjs.com/package/is-generator-function)    
Is this a native generator function
* [koa-compose](https://www.npmjs.com/package/koa-compose)    
Compose middleware
* [koa-convert](https://www.npmjs.com/package/koa-convert)    
Convert koa legacy ( 0.x & 1.x ) generator middleware to modern promise middleware ( 2.x )
* [koa-is-json](https://www.npmjs.com/package/koa-is-json)    
Check if a body is JSON
* [mime-types](https://www.npmjs.com/package/mime-types)    
The ultimate javascript content-type utility
* [on-finished](https://www.npmjs.com/package/on-finished)    
Execute a callback when a HTTP request closes, finishes, or errors
* [only](https://www.npmjs.com/package/only)    
Return whitelisted properties of an object
* [parseurl](https://www.npmjs.com/package/parseurl)    
Parse a URL with memoization
* [statuses](https://www.npmjs.com/package/statuses)    
HTTP status utility for node
* [type-is](https://www.npmjs.com/package/type-is)    
Infer the content-type of a request
* [vary](https://www.npmjs.com/package/vary)    
Manipulate the HTTP Vary header


 
