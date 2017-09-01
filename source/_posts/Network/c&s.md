---
title: cookie和session那点事
categories: [其他,网络]
tag: [网络]
date: 2017-08-9 13:00:00
comments: true
---
# Cookie 和 Session那么一点事


cookie最终还是存在磁盘上的，比如chrome的cookie存放位置    
C:\Users\你当前用户名\AppData\Local\Google\Chrome\User Data\Default\


大家都知道document.cookie是可以读取和设置的，这就说明cookie是可以伪造的。 
不用怕， Secure和HttpOnly为你保驾护航，当然这两个是用在服务端的。  

查看cookie可以设置的属性可以查看 
[Set-Cookie - HTTP \| MDN ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)  

 
#### Secure属性：
 
当设置为true时，表示创建的 Cookie 会被以安全的形式向服务器传输，也就是只能在 HTTPS 连接中被浏览器传递到服务器端进行会话验证，如果是 HTTP 连接则不会传递该信息，所以不会被窃取到Cookie 的具体内容。
 
#### HttpOnly属性：
 
如果在Cookie中设置了"HttpOnly"属性，那么通过程序(JS脚本、Applet等)将无法读取到Cookie信息，这样能有效的防止XSS攻击。




借用知乎某个仁兄的话
#### cookie和session关联   
1. session 在服务器端，cookie 在客户端（浏览器）    
2. session 默认被存在在服务器的一个文件里（不是内存）    
3. session 的运行依赖 session id，而 session id 是存在 cookie 中的，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效（但是可以通过其它方式实现，比如在 url 中传递 session_id）   
4. session 可以放在 文件、数据库、或内存中都可以。    
5. 用户验证这种场合一般会用 session 因此，维持一个会话的核心就是客户端的唯一标识，即 session id   


## 额外的话
* 你从开发者工具，直接删除存在cookie里面的session id，刷新页面，session id是会更新的，被认为是新的会话，同理，你在某个网站登录了，强行删除session id，刷新页面会回到登录页面的。
* 存在cookie里面的session id 的key名字是可以修改的，曾今我们就发生过两个网站key值相同，会乱串，甚至会网站奔溃的情况
* 不同浏览器当然是不共享cookie的
* cookie是每次http请求都会附带上的，所以尽量少
* cookie同一个域名下的用户是共享cookie的
* cookie是可以禁用的
* cookie不允许跨域的，通过设置，二级域名可以使用


>[Set-Cookie - HTTP | MDN ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)  
>[Cookie/Session机制详解](http://blog.csdn.net/fangaoxin/article/details/6952954)      
>[禁止js获取sessionid](http://blog.sina.com.cn/s/blog_67196ddc0102v46w.html)    
>[常用的本地存储——cookie篇](https://segmentfault.com/a/1190000004743454)     
>[细说cookie](http://www.cnblogs.com/fish-li/archive/2011/07/03/2096903.html)    
>[COOKIE和SESSION有什么区别](https://www.zhihu.com/question/19786827)
