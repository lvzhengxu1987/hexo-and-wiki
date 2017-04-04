---
title: openvpn windows端在linux上交叉编译
date: 2016-06-13 21:43:50
tags: openvpn 
categories: tools
---
openvpn windows 交叉编译，在centOS 6.4环境，经过一天的波折，总算成功了。现将步骤做一下简单的总结。

写在前面：有时遇到命令缺失导致编译失败，然后要重新开始，此时一个是要把缺失的软件安装上去，还有个技巧就是把已经放了源代码的tmp和sources先备份下，然后等把缺失的命令装好以后，把之前的tmp和sources删除，然后用cp命令把刚才备份的文件夹在复制过来，用这个方法保证编译程序不会有问题，之前遇到一个问题就是有个o文件编译有问题，再接着编译的时候，死活过不去，很烦人。

<!--more-->
<a href="#refer0001" target="_top">[参考网址]</a>

1. 下载openvpn必要的安装包，由于GFW的存在，只能通过别的方式将需要的包下载，这些包是<pre>easy-rsa-2.2.0_master.tar.gz
tap-windows-9.21.1.exe
openvpn-2.3.6.tar.gz
lzo-2.08.tar.gz
openssl-1.0.1j.tar.gz
pkcs11-helper-1.11.tar.bz2
tap-windows-9.21.1.zip
openvpn-gui-6.tar.gz</pre>在openvpn的源代码中，有这么一段话<pre>BUILDING OPENVPN FOR WINDOWS
Official OpenVPN Windows releases are cross-compiled on Linux using the
openvpn-build buildsystem:
   https://community.openvpn.net/openvpn/wiki/BuildingUsingGenericBuildsystem
First setup the build environment as shown in the above article. Then fetch the
openvpn-build repository:
   git clone https://github.com/OpenVPN/openvpn-build.git
Review the build configuration:
   openvpn-build/generic/build.vars
   openvpn-build/windows-nsis/build-complete.vars
Build (unsigned):
   cd openvpn-build/windows-nsis
   ./build-complete
Build (signed):
   cd openvpn-build/windows-nsis
   ./build-complete --sign --sign-pkcs12=<pkcs12-file>\
   --sign-pkcs12-pass=<pkcs12-file-password> \
   --sign-timestamp="<timestamp-url>"</pre>从git代码库中获取openvpn build后，把上面提到的build和build-complete做一下修改，将里面的下载功能取消<a href="#image0001" target="_top">[见图]</a>，然后在openvpn-build/windows-nsis/目录中mkdir tmp sources,将easy-rsa-2.2.0\_master.tar.gz tap-windows-9.21.1.exe它俩放到tmp目录，将剩下几个压缩包放到sources中。到此为止。
2. 安装python 2.7 、 scons和makensis，其实除了这几个，还有别的软件包也会在下面一并说明，不过其他的都很好安装，如果在openvpn build过程中出现了因为某个命令不存在导致build失败，只要把缺少的软件包安装完毕就可以了，上面三个软件包是比较麻烦的，所以单独说。安装python2.7 是因为scons这个工具需要这个版本以上的，如果不使用Python2.7，它的提示消息说会有不明原因的错误产生，为了少死一些脑细胞，所以就安装吧。 安装python2.7首先要下载它的源码包<pre>wget http://www.python.org/ftp/python/2.7.3/Python-2.7.3.tar.xz</pre>然后解压缩，进入Python-2.7.3中，执行<pre>././configure --prefix=/usr/local && make && make install</pre>执行完毕后，python2.7就被安装到/usr/local里面了，可以去目录看看<pre>
[root@yxzc bin]# pwd
/usr/local/bin
[root@yxzc bin]# ls p*
pip  pip2  pip2.7  pydoc  python  python2  python2.7  python2.7-config  python2-config  python-config</pre> 可以看到python2.7已经安装完毕，接下来修改/usr/bin中的链接，<pre>export PATH="/usr/local/bin:$PATH"
or 
mv /usr/bin/python /usr/bin/python2.6.6
ln -s /usr/local/bin/python2.7  /usr/bin/python
\# 检查
[root@dbmasterxxx ~]# python -V
Python 2.7.8
[root@dbmasterxxx ~]# which python 
/usr/bin/python</pre>然后就要安装setuptools和pip,<pre>wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2
tar -xvf setuptools-1.4.2.tar.gz
cd setuptools-1.4.2
\# 使用 Python 2.7.8 安装 setuptools
python2.7 setup.py install
curl https://raw.githubusercontent.com/pypa/pip/master/contrib/get-pip.py | python2.7 -</pre>在以上步骤做完后，python升级这个动作基本就是做完了，但是centOS6.4的yum是不支持python2.7的，所以用 which yum命令找到yum(就是一个脚本)后，用vim打开它，把#!/usr/bin/python改成 #!/usr/bin/python2.6.6(为什么要改成2.6.6，是因为我在之前的命令中把python用mv命令改成了Python2.6.6)，这样yum就可以使用了，在网上找资料的时候也见过ibus失效的，原理和yum一样，不支持python2.7，在/usr/bin/中的ibus-setup，/usr/libexec/ibus-ui-gtk  这个文件的最后一行，有exec python /usr/share/ibus/setup/main.py $@，把python改成python2.6.6把。<br><br>
python2.7安装完毕后，就可以安装scons了，这个工具就像automake这类的工具，写完源代码后，能自动识别并组织工程结构并编译，直至最后的可执行程序。安装scons比较简单，下载scons的python包后，解压缩，执行python setup.py build 和 python setup.py install<br><br>
makensis也是一个比较难安装的软件，NSIS is a free scriptable win32 installer/uninstaller system that doesn't suck and isn't huge.安装这个软件有如下几个步骤<pre>\#Make sure having installed gcc
yum install gcc.i386
\#Make sure having installed lisbstdc++
yum install libstdc++-devel.i386
\#Make sure having installed g++
yum install gcc-c++.i386
\#下载nsis的包，包括nsis sources和 nsis pre-compiled binary release 
\#下载地址http://sourceforge.net/projects/nsis/files/NSIS%202/2.46/
\#下载里面的nsis-2.46.zip包和nsis-2.46-src.tar.bz2包，也可以下载别的版本试试
cd /root/packages
tar xvfj nsis-2.xx-src.tar.bz2
unzip nsis-2.xx.zip
cd nsis-2.xx-src
scons SKIPSTUBS=all SKIPPLUGINS=all SKIPUTILS=all SKIPMISC=all NSIS\_CONFIG\_CONST\_DATA\_PATH=no PREFIX=/root/packages/nsis-2.xx install-compiler
cp build/release/makensis/makensis /root/packages/nsis-2.xx</pre>注意刚开始安装的软件gcc.i386、libstdc++-devel.i386、gcc-c++.i386这些，他们都是用来编译32位的。接下来的步骤是<pre>
\#rm这步可有可无，如果以前没有安装过nsis，那就没必要
rm -rf /usr/local/nsis
cp -r /root/packages/nsis-2.xx /usr/local/nsis
cd /usr/local/nsis
ln -s . bin</pre>最后不要忘记加环境变量/usr/local/nsis/bin

3. 接下来就可以回到1步骤中的到此为止了，可以执行cd openvpn-build/windows-nsis  ./build-complete了
4. 嗯，可以把编译完毕的exe文件拿到windows环境试试了。

<a name="refer0001" style="color: black">网址</a>
[python update](http://ruiaylin.github.io/2014/12/12/python%20update/)
[Installing NSIS all platforms](https://cwiki.apache.org/confluence/display/DIRxSBOX/Installing+NSIS+-+All+platforms)
[error-gnu-stubs-32-h-no-such-file-or-directory](http://stackoverflow.com/questions/7412548/error-gnu-stubs-32-h-no-such-file-or-directory-while-compiling-nachos-source)


<a name="image0001" style="color: black">图片</a>
{% img /site_files/2015-11-06_165331.png %}
{% img /site_files/2015-11-06_164109.png %}

