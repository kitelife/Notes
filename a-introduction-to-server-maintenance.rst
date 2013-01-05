SI组服务器维护指南
==================

:作者:
    夏永锋

:版本:
    初稿，2012/04/01

一.核心思想
-----------

保持谨慎，时刻备份！
^^^^^^^^^^^^^^^^^^^^


**最佳实践:**

1. 在服务器上进行任何 **增删改** 操作之前，都应该自己的电脑(和服务器安装一样的操作系统Debian)上先试验一遍。

2. 修改任何文件之前，都使用cp filename filename.bak对文件进行备份。

二.服务器管理员的职责
---------------------

**1. 及时解决服务器存在的问题，保证正常服务**

**2. 灵活地满足用户的服务新需求**

另外，管理员应将管理过程中遇到的问题，原因，解决方案等信息详细记录在 `网络管理员日志 <http://202.120.40.124/index.php/Lab:Network:NoteBook>`_ 中，以为后来的管理员提供帮助，以快速地解决同样的问题。

三.服务器的概念
---------------

对于"服务器"的概念可以从三个层面上理解:

1.硬件
^^^^^^
一般人对于服务器的理解都是在这个层面上的。其实从硬件层面上来说，服务器和普通的PC没什么区别，唯一的区别可能就是服务器的硬件配置一般都比PC好，或者服务器并不需要显卡什么的。

2.软件
^^^^^^
比如，http服务器软件(Apache,Nginx,httpd等)，FTP服务器软件等。这些软件时刻准备着响应客户端软件的服务请求。你也可以自己写个简单的服务器程序。

3.服务进程
^^^^^^^^^^
每个服务器软件都会启动一个或多个进程来响应服务请求。每个进程一般监听一个端口。比如http服务的默认端口为80，ftp服务的默认端口为20,21。这些进程会监听对应的端口，没有服务请求时就睡眠。

在本文档中主要介绍一些服务器软件的安装, 配置, 服务器网络配置, 网络安全以及其它相关问题。

四. SI组服务器硬件配置
-----------------------

- **CPU**: Intel(R) Pentium(R) 4 CPU 2.80GHz
  
  - CPU信息查看命令: cat /proc/cpuinfo


- **Memory**: 512MB

  - 内存信息查看命令: cat /proc/meminfo


- **Disk**: /dev/hda: 80.0 GB; /dev/sda: 1000.2 GB; /dev/hdb: 200.0 GB

  - 硬盘信息查看命令：df -h 或 fdisk


- PCI信息查看命令: lspci

- USB信息查看命令: lsusb

另外:

- 操作系统: **Linux debian 2.6.26-2-686**

  - 查看命令: uname -a

五. SI组服务器提供的服务
-------------------------
1. Web服务---Apache + wiki
^^^^^^^^^^^^^^^^^^^^^^^^^^

嵌入式实验室的 `wiki <http://www.mediawiki.org/wiki/MediaWiki/zh>`_ 由 `Apache <http://www.apache.org>`_ 驱动，即Apache监听某个端口(默认为80)，当有用户浏览器请求访问某个网页时，Apache就处理这个请求，将相应网页的内容返回给用户浏览器或者执行其他操作。

Debian(目前实验室服务器使用的Linux系统)下安装Apache，只要在命令行里执行 **apt-get install apache2** 即可。

安装程序会自动生成/etc/apache2/这个目录存放配置文件。一般安装成功后，Apache会自动运行。如果没有运行，可执行命令 **/etc/init.d/apache2 start** 来启动。

/etc/apache/的目录结构如下：

|    .
|    ├── apache2.conf
|    ├── conf.d
|    │   ├── charset
|    │   ├── javascript-common.conf -> /etc/javascript-common/javascript-common.conf
|    │   ├── localized-error-pages
|    │   ├── other-vhosts-access-log
|    │   └── security
|    ├── envvars
|    ├── httpd.conf
|    ├── magic
|    ├── mods-available
|    │   ├── actions.conf
|    │   ├── authn_alias.load
|    │   ...
|    │   ├── mime_magic.load
|    │   ├── proxy_connect.load
|    │   ├── proxy_ftp.conf
|    │   ├── proxy_ftp.load
|    │   └── vhost_alias.load
|    ├── mods-enabled
|    │   ├── alias.conf -> ../mods-available/alias.conf
|    │   ├── cgid.conf -> ../mods-available/cgid.conf
|    │   ├── cgid.load -> ../mods-available/cgid.load
|    │   ...
|    │   ├── status.conf -> ../mods-available/status.conf
|    │   └── status.load -> ../mods-available/status.load
|    ├── ports.conf
|    ├── sites-available
|    │   ├── default
|    │   └── default-ssl
|    └── sites-enabled
|        └── 000-default -> ../sites-available/default

5 directories, 130 files

Apache2配置文件和以前的版本有所不同，以前的主配置文件是httpd.conf，在apache2.0中httpd.conf是个空文件，apache2中的apache2.conf使用#include指令对配置文件进行了分离。

ports.conf是服务器监听端口的设置。conf.d目录下为主配置文件的一些补充，比如默认字符集的设置charset。

从文件类型为symbolic link我们可以知道，sites-avaliable目录下是配置好的站点的配置文件，sites-enabled下则是指向这配置文件的符号链接。之所以这样放置配置文件，是了方便配置apache驱动多个网站。

管理员需要熟悉上述说明的配置文件。

Apache默认设置的网站根目录为/var/www/，可以通过修改sites-available/default中DocumentRoot选项。

如果希望将apache的默认监听端口80修改为其他端口，可修改ports.conf中的NameVirtualHost与Listen两项。但不是万不得已，主站的端口不要修改，因为修改了默认端口，不做特别设置，那用户每次访问wiki都需要在ip后加:port，很麻烦。

关于Apache的详细内容参考: `Apache2.2中文文档 <http://lamp.linux.gov.cn/Apache/ApacheMenu/>`_ 

由于MediaWiki的大部分内容是需要存放在数据库里的，所以实验室服务器安装了MySQL，具体情况见下节。

**注:** 实验室wiki(MediaWiki)是用PHP写的，但Apache默认并不是支持PHP，所以安装Apache后还需要配置。

**注:** 服务器管理并不需要安装Apache，甚至只要不出现问题或者没有新的需求就不要去动服务器软件及其配置。但管理员需要理解其原理，所以管理员在开始的时候应该多阅读Apache文档，在自己的电脑上多实践，以便服务器有突发问题时有能力应付。

2. 数据库服务---MySQL
^^^^^^^^^^^^^^^^^^^^^

PHP对于MySQL的支持很好，MySQL本身也易用，性能也不差。对于实验室Wiki这样网络访问量的应用来说，MySQL做数据存储足够。

**apt-get install mysql-server** 安装MySQL过程中一般需要为root用户设置密码。安装完毕后，如果MySQL没有自动运行，则运行 **/etc/init.d/mysql start** 来启动。

如果已经登录到服务器上，则可通过命令 **mysql -u root -p** ，输入密码后，进入MySQL命令行。如果是远程登录，则需使用 **mysql -h 服务器IP -u root -p** 来登录(当然前提是已经在自己电脑上装了mysql client)。
另外，你也可以在自己电脑上通过MySQL图形化客户端(比如Navicat Lite for MySQL)访问服务器数据库，这样确实比较方便易用。但作为服务器管理员还是需要熟悉命令行的哈，毕竟服务器一般都没有图形界面的。

关于MySQL的详细信息参考: `MySQL 5.1参考手册 <http://dev.mysql.com/doc/refman/5.1/zh/index.html>`_

关于数据库备份，恢复的内容见"自动备份"一节。

**注:** 管理员一般不需要去管MySQL中存了什么东西，怎么存的。

**注:** 为了让PHP支持MySQL,一般还需安装PHP-MySQL这个模块，还是用apt-get安装哈。

3. FTP服务---vsftpd
^^^^^^^^^^^^^^^^^^^^

由于FTP协议出现得比较早，所以缺乏安全方面的考虑。但对于内部使用或者文件共享却是很方便的。

实验室的FTP服务使用的 `vsftpd <http://security.appspot.com/vsftpd.html>`_ 。其配置文件为/etc/vsftpd.conf以及/etc/vsftpd/目录下针对每个用户名的配置文件以控制其访问权限。

FTP服务默认控制端口21，但实验室ftp服务的控制端口修改为了2121，原因不详，可能是出于安全考虑。配置选项见/etc/vsftpd.conf的listen_port一项。

FTP服务的根目录为/var/ftp，每个用户名对应的根目录可不一样，可通过/var/vsftpd/下其对应的配置文件查看local_root选项。另外chroot_local_user=YES一项很重要，它使得用户登录FTP后的初始目录为local_root,且限制用户不能进入local_root的祖先目录，只能进入local_root的子目录。

添加FTP用户的步骤参见: `wiki网络管理员日志 <http://202.120.40.124/index.php/Lab:Network:NoteBook>`_

关于FTP的工作原理以及vsftpd配置的具体信息可参考:**《鸟哥的Linux私房菜---服务器架设篇》** 第21章"vsFTPd文件服务器"(这本书应该常备身边)

**注:** 目前ftp服务的主要问题是中文乱码问题，这是因为不同的操作系统以及不同的个人电脑的默认中文编码是不一样的，那中文以及特殊符号就非常容易出现乱码。这个问题尽量不要去碰，不那么容易解决。

4. FTP搜索功能
^^^^^^^^^^^^^^

由于实验室ftp服务文件数目的增多，如果时间长了，你忘了某个文件的具体位置，要找到这个文件那真是一件浪费时间的事情。这时搜索的价值就体现出来了。

有些ftp客户端提供搜索功能，但这种搜索一般是遍历ftp目录，这种即时的遍历会极大地增加服务器的负载。所以应该把ftp的搜索功能分为异步的两个步骤---索引与搜索。

1. 索引过程可以放在服务器负载较低的临晨进行，遍历ftp目录，将文件的绝对路径(绝对路径唯一地区分一个文件)处理后形成一个有效的ftp访问URL存放入数据库。

2. 搜索过程只是查找数据库。这样能极大地减小服务器的负载。

---

索引功能由python脚本实现，见/root/IndexFTP/目录:

|    .
|    ├── listDir.py
|    ├── LogForListFTP.txt
|    ├── operateLog.py
|    ├── operateLog.pyc
|    ├── operationOnMySQL.py
|    └── operationOmMySQL.pyc

0 directories, 6 files

listDir.py为主脚本文件; operateLog.py中的函数被listDir.py调用，会往LogForListFTP.txt记录一些日志信息; operationOnMySQL.py中是MySQL数据库相关操作的类方法，在listDir.py中被实例化调用。.pyc为后缀名的文件是python为了提高程序性能，为每个被主脚本调用的脚本文件自动生成的二进制代码文件。

listDir.py脚本在临晨自动运行，这是通过cron守护进程实现的。(关于cron的详细信息请查看man手册:man cron)

目前的设置是临晨两点自动运行，可通过crontab -l命令查看：

|   # m h   dom mon dow command
|     0 1   *   *   *   /root/backup_wiki
|     * *   *   *   *   /root/keepconnect >> /dev/null
|     0 2   *   *   *   /root/IndexFTP/listDir.py >> /dev/null

如上输出最后一项，意思为每天的临晨两点整运行/root/IndexFTP/listDir.py脚本程序，并将程序的标准输出重定向到/dev/null这个空文件，表示丢弃标准输出。

---

搜索部分由PHP实现，见/var/www/wiki/ftpsearch/目录。

目前只是将用户的输入作为一个字符串整体进行数据库查找。对于实验室的ftp服务来说这已经足够。

访问202.120.40.124/ftpsearch，搜索某个关键字，点击返回结果中每一项的超链接，输入用户名密码即可下载。

5. 版本控制服务---SVN(subversion)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

版本控制服务主要是为了方便各项目组成员之间协作，以及代码共享。

**安装配置:**

1. apt-get install openssl subversion libapache2-svn

2. 新建svn目录(/home/svn)，配置目录所有者(www-data)以及权限

   $ mkdir /home/svn
   
   $ chown -R www-data:www-data /home/svn/
   
   $ chmod -R 770 /home/svn/

3. 创建svn用户密码配置文件: /etc/apache2/dav_svn.passwd

   $ /usr/bin/htpasswd -c /etc/apache2/dav_svn.passwd svnadmin
   
   New password:
   
   Re-type new password:
   
   Adding password

   *密码文件默认加密方法:CRYPT encryption,密码文件格式: 用户名:密码*

4. 创建svn目录权限配置文件: /etc/apache2/dav_svn.authz

5. 配置/etc/apache2/mods-available/dav_svn.conf

   创建svn location, 指定svn目录，认证方式，认证信息;
   
   指定dav_svn.passwd用户密码配置文件路径;
   
   指定dav_svn.authz目录权限配置文件路径。

6. 创建svn版本库

   $ su www-data
   
   $ svnadmin create /home/svn/repos

7. 配置完成，重新启动apache2服务

   $ /etc/init.d/apache2 restart

现在就可以通过浏览器访问 http://202.120.40.124/svn/repos/ 来浏览代码库了。

也可以将其clone到本地: svn co http://202.120.40.124/svn/repos labsvn

对代码做出修改后，通过 svn commit -m "说明信息",经过验证就能成功提交修改。

**管理**:

1. 新建用户(htpasswd SHA加密方法, 参数: -s)

   $ /usr/bin/htpasswd -s /etc/apache2/dav_svn.passwd 用户名

2. 删除用户(vi/vim编辑)

   $ vim /etc/apache2/dav_svn.passwd

   查找指定用户名: /用户名
   
   删除指定用户的行: dd
   
   保存退出: wq

   以及修改/etc/apache2/dav_svn.authz中相关内容。

**说明**

目前svn服务的设置是：只创建一个repos版本库，不同的项目以不同的文件夹的方式区分全部放在repos中。

repos库中现有三个项目: **car4** -- *第4版小车的代码*; **car5** -- *第5版小车的代码*; **simpleFTPsearch** -- *FTP搜索服务的代码*

**注:** 由于实验室人员使用svn服务的需求比较小，所以以后可以考虑取消svn服务。

6. SSH服务---openSSH
^^^^^^^^^^^^^^^^^^^^^

SSH服务一般是为了方便管理员远程登录服务器进行管理操作。

1. 安装ssh-server

   apt-get install openssh-server

2. 启动ssh服务
   
   $ /etc/init.d/ssh start

   Starting OpenBSD Secure Shell server: sshd.

3. 确认ssh-server已经正常工作

   $ netstat -tlp

   如果输出中有如下所示的一行内容，则说明ssh-server已经在运行

   tcp  0   0   *:ssh   *:*     LISTEN  -

4. 远程登录服务器

   $ ssh -l 用户名 服务器IP

   接下来会提示输入密码，然后就能成功登录到服务器上了。

六.服务器网络配置
-----------------

连接因特网
^^^^^^^^^^

1. 配置文件

    Debian中，网络配置文件为 **/etc/network/interfaces** 和 **/etc/resolv.conf**

    interfaces的内容一般如下所示:

    **auto lo**
    
    **iface lo inet loopback**

    **auto eth0  #eth0为网卡的名称**

    **iface eth0 inet static**

    **address 服务器IP**

    **netmask 掩码**

    **gateway 网关**

    resolv.conf中设置DNS服务器IP:

    **nameserver DNS服务器的IP**
    
2. 重启网络

    $ /etc/init.d/networking restart

IP共享---NAT
^^^^^^^^^^^^

嵌入式实验室服务器除了提供上述服务外，还需要为 **3107** 房间内所有的计算机提供NAT服务，以访问外网。因为每个房间只有一个外网网口，服务器连上之后，正好复用为其他机器提供NAT服务。

为了提供NAT服务，需要设置两步:

1. 设置网络数据转发标志位

   echo "1" > /proc/sys/net/ipv4/ip_forward

2. 设置防火墙iptables

   iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE

   iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth1 -j MASQUERADE

把这两步一起写到一个脚本ip.sh中，每次NAT没有正常工作，只要执行这个脚本就行了。服务器上这个脚本在/root/ip.sh处。

关于NAT的原理，以及上述设置的解释请参考： **《鸟哥的Linux私房菜---服务器架设篇》第11章：Linux防火墙与NAT主机** 

七.网络安全 --- Snort
----------------------

作为服务器，可能会遭到恶意入侵(额，入侵有善意的么？)。所以需要一款工具来及时地发现这样的入侵，并做出适当的反应，以减小入侵攻击的损失以及为服务器恢复，跟踪入侵来源提供帮助。

`Snort <http://www.snort.org>`_ 是一套开放源码的网络入侵预防软件与网络入侵检测软件。

snort有三种工作模式：嗅探器，数据包记录器，网络入侵检测系统。
    
    1. 嗅探器模式仅仅是从网络上读取数据包并作为连续不断的流显示在终端上。

    2. 数据包记录器模式把数据包记录到硬盘上。

    3. 网络入侵检测模式是最复杂的，而且可配置的。可以让snort分析网络数据流以匹配用户定义的一些规则，并根据检测结果采取一定的动作。

在服务器上安装好snort之后，就需要自定义规则，snort官网提供了一套 `规则 <http://www.snort.org/snort-rules/?>`_ 。

Debian系统上，snort的配置文件以及规则文件，都放置在 **/etc/snort/** 目录下。

启动|重启|关闭snort命令: /etc/init.d/snort start|restart|stop

如果snort运行遇到问题，可根据提示在/var/log/daemon.log, /var/log/syslog中查找问题的原因。

关于snort的详细信息请查看 **snort官方文档**

八.自动备份
-----------

为了应对各种意外情况，即使服务器出现了故障，我们也能迅速地恢复服务，我们需要对一些重要数据进行备份。服务器上有一项cron任务对wiki以MySQL数据库数据进行自动备份。

    $ crontab -l
        
    # m h  dom mon dow   command
    
    0 1 * * * /root/backup_wiki
    
    \* \* \* \* \* /root/keepconnect >> /dev/null
    
    0 2 * * * /root/IndexFTP/listDir.py >> /dev/null

    最后一项前面已经说过。 
    
    **/root/backup_wiki** 一项即为自动备份任务。具体内容参考/root/backup_wiki脚本文件内容

另外，也可以周期地刻录光盘来对服务器的重要数据进行备份。

**注：** /root/keepconnect >> /dev/null 一项是因为服务器网卡老化，比较容易断网。通过每分钟ping两次某个外网地址来使网卡保持 **激活** 状态

九.其他
-------

Important Commands
^^^^^^^^^^^^^^^^^^

1. **apt-cache search** : 查询软件包
    
2. **apt-get install/purge** : 安装/卸载软件包
    
3. **grep** 正则表达式查找；比如: grep 'python' -R . ,在当前目录递归查找含有关键字'python'的文本文件，以及'python'在文本文件所在的行。

4. **tail/head -n 5 sample.txt** : 查看sample.txt文件的头5行/尾5行

5. **scp** : 远程拷贝命令。(当你自己用的linux系统，那使用scp在个人电脑与服务器之间拷贝文件非常方便)

6. **ps -e** : 查看所有的进程

7. **top** : 进程的内存与CPU消耗动态
