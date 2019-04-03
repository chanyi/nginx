# nginx 

## 1、基础

### 1.1、编译与安装

#### 1.1.1下载

下载网址官网：http://nginx.org/   。现在的时候选择稳定版（stable）

#### 1.1.2安装

进入指定目录： cd /usr/local/src/

下载最新最稳定版的nginx：wget http://nginx.org/download/nginx-1.14.2.tar.gz

解压：tar -zxvf nginx-1.14.2.tar.gz 

安装：cd nginx-1.14.2/

​	   ./configure --prefix=/usr/local/nginx

​	    执行上面可能会编译错误提示没有gcc，则先应该安装gcc，使用命令：apt-get  build-depgcc，如果apt找不到gcc库，那么需要更新系统的源，可以使用中科大的源：修改文件:vim /etc/apt/sources.list,将原来的源删除，换上下面的源。

```
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
```

​	   然后更新执行：apt-get update，apt-get upgrade

​	执行上述操作后即可安装gcc：apt-get install gcc。

​	然后再次执行编译，发现还是报错：./configure: error: the HTTP rewrite module requires the PCRE library.

​	根据上面的错误安装pcre库：有的系统找不到pcre，执行apt-get install pcre和apt-get install pcre-devel失败，那么需要执行：apt-get install libpcre++-dev才行

​	然后再次执行编译，发现报错：./configure: error: the HTTP gzip module requires the zlib library.

​	根据报错，安装zlib。同样直接安装apt-get install zlib找不到这个包，需要执行：sudo apt-get install zlib1g-dev。所以可以通过ubuntu software center官网（www.ubuntu.com）来找包对应的可下载的名字。

​	然后再次执行编译，这次正确执行。

​	下一步即可执行：make && make install（make若是没有安装，则执行：apt-get install make）

#### 1.1.3启动

​	进入目录：/usr/local/nginx/，目录下的四个文件夹分别是：

​		conf：配置文件

​		html：网页文件

​		logs：日志文件

​		sbin：主要的二进制文件

​	启动命令：./sbin/nginx 。输入网址即可验证（http://192.168.104.52/），如果启动提示端口被占用，则需要kill掉80端口对应的程序。 （查看端口被那个进程占用的命令是：netstat -antp）

#### 1.1.4信号量

​	nginx使用信号量来操作

​	操作规范：kill -信号量 进程号，例如：kill -INT 234234

| 信号量   | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| TERM,INT | 立即关闭，及时用户还在访问也立即关闭                         |
| QUIT     | 优雅关闭（graceful shutdown）,等待请求结束                   |
| HUP      | 改变了配置文件，平滑的重读配置文件，（例如修改了根目录的访问index.html文件，在重启后，重新加载修改后得index.html） |
| USR1     | 重读日志，在日志按照月/日分割时有效（例如：需要将access.log文件被分为access.log.2019-03，然后新的日志写在新建的access.log。这种情况下，需要使用到USR1。执行过程为：先备份access.log文件为access.log.2019-03,执行：mv access.log access.log.2019-03,然后新建access.log文件，执行：touch access.log,最后执行USR1：kill -USR1 进程号） |
| USR2     | 平滑的升级（当nginx要升级，但是就的nginx还在执行，这个时候就需要用到USR2） |
| WINCH    | 优雅关闭旧的进程（配合USR2来使用）                           |

**小技巧**:

​	这里的进程号可以不用每次都去查询，可以使用命令：cat logs/nginx.pid获取到，例如：kill -HUP 'cat logs/nginx.pid'

​	也可以使用简化的信号量控制：例如

​	./sbin/nginx -s reload

​	./sbin/nginx -s stop

​	./sbin/nginx -s reopen

​	./sbin/nginx -t -->这个命令可以判断当前的配置文件是否正常。如果有错误，就会显示出来

#### 1.1.5虚拟主机的配置

​	nginx使用信号量来操作





















​	

​	










​	



