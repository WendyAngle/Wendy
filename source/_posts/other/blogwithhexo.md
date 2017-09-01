---
title: Hexo + Github搭建自己的博客
categories: [其他,github]
tag: [github]
date: 2017-08-19 22:00:00
comments: true
---

如题，Hexo + Github搭建自己的博客，本博客就是如此搭建的，   
当前是 Hexo + Github pages + disqus + 不蒜子   

网上有很多，我这里推荐 [hexo从零开始到搭建完整](http://www.cnblogs.com/visugar/p/6821777.html)和[Hexo博客添加SEO-评论系统-阅读统计-站长统计](http://visugar.com/2017/08/01/20170801HexoPlugins/)  
已经基本满足你的需求，我这里主要是记录一些链接地址和自己在弄得时候遇到的一些问题。  

我这里是windows，请注意，请注意    
步骤，
1. 安装Git Bash    
    [Git for Window](https://git-for-windows.github.io/) 
2. 安装NodeJs   
    [Node.js](https://nodejs.org)    
    建议使用最新版本
3. 创建github仓库   
    [github](https://github.com/)
4. 安装Hexo    
    安装完毕后，务必查看官方文档，

    [Hexo](https://hexo.io/zh-cn/)   
    [文档\|Hexo](https://hexo.io/zh-cn/docs/index.html)   
    [指令\|Hexo](https://hexo.io/zh-cn/docs/commands.html)   
    [Themes\|Hexo](https://hexo.io/themes/)   
    [配置\|Hexo](https://hexo.io/zh-cn/docs/configuration.html)   
    [变量\|Hexo](https://hexo.io/zh-cn/docs/variables.html) 

    配置还是有一些小问题的，官方在[配置\|Hexo]里面提到   
    ```
    如果您的网站存放在子目录中，例如 http://yoursite.com/blog，
    则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。
    ```
    我的情况也是如此，可是配置过后，Share和留言的url并不是正确的，缺少 blog这一段路径，没有办法，自己做了一些修改 ,    
    主要是修改了 post.permalink 为  config.url +  config.root +  post.path
    和 url_for(post.path)
    ```
    <footer class="article-footer">
      <a data-url="<%- config.url +  config.root +  post.path %>" data-id="<%= post._id %>" class="article-share-link"><%= __('share') %></a>
      <% if (post.comments && config.disqus_shortname){ %>
        <a href="<%-  url_for(post.path) %>#disqus_thread" class="article-comment-link"><%= __('comment') %></a>
      <% } %>
      <%- partial('post/tag') %>
    </footer>
    ```

   * Theme默认 hexo init的时候是landscape，你可以到 [Themes\|Hexo](https://hexo.io/themes/)去选择自己喜欢的，下载下来修改配置就行。
   * Hexo主要的配置都在_config.yml里面，语法都是 属性名:[空格]值,参考: [配置\|Hexo](https://hexo.io/zh-cn/docs/configuration.html)
   * Hexo 是支持本地调试的执行 hexo server，默认端口是4000, http://localhost:4000, 有时候本地调试一些东西，因为cors策略，是没法调试通的，怎么办，修改host啊
   * Hexo 是支持直接发布的，hexo deploy,这个需要你配置ssh，[hexo从零开始到搭建完整](http://www.cnblogs.com/visugar/p/6821777.html)里面有提到，可以参考[git ssh配置和使用](https://segmentfault.com/a/1190000002645623)
   * 博客编写是在 souce/_post下面，默认是直接在这下面的，你可以自己再创建目录
   * 博客的日期默认应该是文件创建日期，曾有人说过，换电脑后，全部为同一天了，怎么解决，在你的博客头部添加date, 如下，更多变量查看[变量\|Hexo](https://hexo.io/zh-cn/docs/variables.html)  
   
```
---
title: Hexo + Github搭建自己的博客
categories: github
tag: [github,blog]
date: 2017-08-19 22:00:00
comments: true
---
```
   
5.添加评论功能   
评论插件,多说和网易云跟帖阵亡 ,更多查看[第三方评论系统推荐](https://blog.shuiba.co/comment-systems-recommendation)   
[disqus](https://disqus.com/)   
[来必力](https://livere.com/)   
[畅言](http://changyan.kuaizhan.com/)    
[友言](http://www.uyan.cc/demo)   
[gitment](https://github.com/imsun/gitment)   
[QingQing](https://qingqing.com/post/1021205/time#next)     
我选择disqus别问我为什么，
具体可以参考[GithubPages + Hexo + Disqus博客教程](http://www.jianshu.com/p/91c01f236846)

6.统计   
[Google Analysis](https：//www.google.com/intl/zh-CN/analytics/)   
[CNZZ](http://web.umeng.com)   
[百度统计](http://tongji.baidu.com)   
[leancloud](https://leancloud.cn/)   
[不蒜子](http://ibruce.info/2015/04/04/busuanzi/)  
[related-popular-posts](https://github.com/tea3/hexo-related-popular-posts)  
我选择不蒜子，具体参考[不蒜子](http://ibruce.info/2015/04/04/busuanzi/) 
    

## 更换theme 
在[Themes\|Hexo](https://hexo.io/themes/) 找到你喜欢的Theme，点击图是预览，点击下面的文字是进入对应的github仓库。   
我更换到 [hueman](https://github.com/ppoffice/hexo-theme-hueman)   
1. 打开本地项目，执行git clone https://github.com/ppoffice/hexo-theme-hueman.git themes/hueman   
2. 用IDE打开 themes/hueman/_config.yml.example,去掉后缀.example
3. 修改_config.yml,参考[Configuration](https://github.com/ppoffice/hexo-theme-hueman/wiki/Configuration)  

* hueman 自带是支持disqus评论功能，只需修改_config.yml配置disqus_shortname就行，如没有就添加，本身是支持 disqus,多说,isso,facebook,友言，畅言的   
* hueman 默认的首页，每个博客是带图片的，你可以打开themes/hueman/_config.yml修改customize:thumbnail    
* 有时候，你觉得内容展示区域太小了，怎么办，打开themes/hueman/source/css/_variables.styl,找到 container-inner-max-width，默认是1100多，我修改为80% ,在高分辨下，可能会有更好的效果  
* 支持搜索，简直没话说，具体参考[Search · ppoffice/hexo-theme-hueman Wiki](https://github.com/ppoffice/hexo-theme-hueman/wiki/Search)  
* 你会发现搜索有个图标，但是点击确没有反应，我不喜欢，找到 themes/hueman/source/js/insight.js,大约220行    
修改` $(document).on('click focus', '.search-form-input'` 为 ` $(document).on('click focus', '.search-form'`，就好了   
* 代码高亮，hueman用的是[HighLight.js](https://github.com/isagalaev/highlight.js),支持的语言可以参考[Css Classes Reference](https://highlightjs.readthedocs.io/en/latest/css-classes-reference.html),这些在[Theme \| Hueman](https://github.com/ppoffice/hexo-theme-hueman/wiki/Theme) 有提到    
* hexo是支持资源文件的，分 **Global Asset Foler** 和 **POST ASSET Folder**, 实现原理居然是每个Post创建一个文件夹，有点醉，具体可以看[Asset Folders](https://hexo.io/docs/asset-folders.html)   





hexo    
[20分钟教你使用hexo搭建github博](http://www.jianshu.com/p/e99ed60390a8)   
[How to setup a blog on github with Hexo](https://zirho.github.io/2016/06/04/hexo/)   
[使用Hexo & Github,搭建属于自己的博客](http://www.cnblogs.com/ld1024/p/5913169.html)    
[给hexo配置上评论和访问量](https://bblove.me/2015/05/30/hexo-setting-with-comments-and-visitors/)    
[hexo从零开始到搭建完整](http://www.cnblogs.com/visugar/p/6821777.html)  
[Hexo博客添加SEO-评论系统-阅读统计-站长统计](http://visugar.com/2017/08/01/20170801HexoPlugins/)      
统计访问次数    
[不蒜子](http://ibruce.info/2015/04/04/busuanzi/)   
[hexo-related-popular-posts](https://github.com/tea3/hexo-related-popular-posts)  
[Hexo统计post阅读次数](http://www.icafebolger.com/hexo/hexopostcount.html)  
[使用LeanCloud平台为Hexo博客添加文章浏览量统计组件](http://crescentmoon.info/2014/12/11/popular-widget/)    
leancloud   
[leancloud](https://leancloud.cn/)   
[LeanCloud JavaScript SDK](https://leancloud.github.io/javascript-sdk/docs/)   
其他   
[搭建个人博客，你需要知道这些](https://zhuanlan.zhihu.com/p/25744686)  
[Hexo搭建博客之博客搜索引擎推广](http://huangnx.com/2016/04/01/blogForSearch/)     
[将hexo博客（github pages）同时同步托管到github和coding.net,解决百度不收录问题](http://blog.jarjar.cn/how-to-deploy-hexo-to-both-github-and-coding/)
[Able to push to all git remotes with the one command?](https://stackoverflow.com/questions/5785549/able-to-push-to-all-Git-remotes-with-the-one-command)

[来必力](https://livere.com/)   
[QingQing](https://qingqing.com/post/1021205/time#next)