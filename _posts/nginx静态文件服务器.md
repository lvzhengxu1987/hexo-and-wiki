---
title: nginx静态文件服务器
date: 2016-06-13 21:50:46
tags: [nginx,web]
categories: tools
---

1. 下载nginx源代码后进行编译，此步骤的configure默认配置即可，不要忘记安装需要的库，还有在make install，配置nginx路径的环境变量
2. 修改nginx默认的配置文件
<!--more-->
   加入这三行代码
   <pre>
   autoindex on;             #开启索引功能  
   autoindex_exact_size off; # 关闭计算文件确切大小（单位bytes），只显示大概大小（单位kb、mb、gb）  
   autoindex_localtime on;   # 显示本机时间而非 GMT 时间</pre>
   修改后的结果如下{% img /site_files/2015-11-02_165303.png %}<br/>
3. 修改后，启动nginx，在配置文件中root参数后面跟的一般就是放置的html文件，可以在那个路径再建立个文件夹，然后放上几个文件，然后用浏览器打开 xxx.xxx.xxx.xxx/dir/,可以看到效果。<br/>{% img /site_files/2015-11-02_165756.png %}<br/>
