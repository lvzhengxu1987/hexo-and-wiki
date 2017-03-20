---
title: 安装hexo
date: 2016-04-17 21:39:30
tags: hexo
---

在一个干净的centos系统上，首先安装node.js和git.在安装node.js前，先把epel装上去，标准版发布的程序中，没有node.js,代码如下: 
`` yum install -y epel-release.noarch;yum install -y nodejs.x86_64 npm.noarch git; ``

然后使用npm安装hexo组件,代码如下:
`` npm config set strict-ssl false;npm install -g hexo-cli; ``

<!--more-->

初始化hexo博客目录
`` hexo init dir;cd dir;npm install;``

安装主题和插件 主题和插件是可选的！
`` cd themes;git clone https://github.com/wzpan/hexo-theme-freemind.git 
npm install hexo-tag-bootstrap --save； 
npm install hexo-generator-search --save
``

然后修改文件夹名称，改成freemind。再把根目录中的_config.yml中主题改成freemind
最后边写文章，生成页面并启动服务器。hexo g;hexo s

此时就可以输入http://xxx.xxx.xxx.xx:4000/来访问你的博客页面了。

不要忘记生成tags和categories,生成方法是 ``hexo n page tags; hexo n page categories``，然后在对应的目录里面将index的内容改下.

在上面的步骤完成后，可以使用nginx做反向代理，这样在访问网站的时候，就不用输入4000这个端口了。
nginx 配置如下
<pre>server {
    listen 80;
    server_name yourdomian.com, www.youdomain.com;

    location / {
		root  /xxxx-you-blog-dir/public/;
        proxy_pass              http://127.0.0.1:4000/;
        proxy_redirect          off;
        proxy_set_header        X-Real-IP       $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        }
  }
</pre>

这样做后，就可以使用 ``nohup hexo s &``将hexo在后台中启动，然后在``service nginx restart``将nginx启动，之后再设置下iptables，就可以让远程服务器上面的程序永久启动了。

在hexo的写好的文章的某处加上 <!--more-->之后，在<!--more-->之前的内容会显示在页面上，而之后的内容只有点开文章后，才能看见。

### 参考链接

[freemind 安装基础步骤]( http://www.hahack.com/codes/hexo-theme-freemind )
[nginx配置]( http://www.tuijiankan.com/2015/05/04/%E9%98%BF%E9%87%8C%E4%BA%91Centos6%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AENodejs%E3%80%81Nginx%E3%80%81Hexo%E6%93%8D%E4%BD%9C%E8%AE%B0%E5%BD%95/ )

