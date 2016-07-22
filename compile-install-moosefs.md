### 实验环境:
- ubuntu 15.04 desktop
   > * **cpu i5-2410、Mem 8G、hdd 1T**
   > * **kvm+qemu 虚拟  6 x ubuntu server 14.04**

### 简介
- MooseFS 简称mfs
- 国内最大使用者豆瓣 1500PB的存储集群
- 具体参照 [who-is-using-moosefs][1]

### KVM 启动错误修复

```shell
     $ virsh net-autostart default
```

##### 服务基本架构
- server1: ip 192.168.122.186 主控
- server2: ip 192.168.122.185 主控备份
- server3: ip 192.168.122.112 web server
- server4: ip 192.168.122.207 chunk server
- server6: ip 192.168.122.237 chunk server
- server5  ip 192.168.122.238 chunk server

##### MooseFS 文件系统安装过程

- 依赖安装
> * apt-get install build-essential
> * apt-get install zlib1g-dev

- 用户组创建
> * groupadd mfs
> * useradd -g mfs mfs

- 编译安装 (主控)

  ```shell
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib --with-default-user=mfs --with-default-group=mfs --disable-mfschunkserver --disable-mfsmount

  make && make install
  ```

- 备份编译

  ```shell
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib --with-default-user=mfs --with-default-group=mfs --disable-mfschunkserver --disable-mfsmount

  make && make install
  ```

- 数据存储编译

  ```shell
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib --with-default-user=mfs --with-default-group=mfs --disable-mfsmaster

  make && make install
  ```

- 客户端编译

  ```shell
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib --with-default-user=mfs --with-default-group=mfs --disable-mfsmaster --disable-mfschunkserver

  pkg-config
  pcap
  make && make install
  ```

- 命令
   - mfsgettrashtime 设置回收时间 把默认的86400改为300秒
	 > *  /usr/local/mfs/bin/mfssettrashtime 64800 /mnt/mfs/test1


- mfscheckfile 检测文件副本复制个数
- /usr/sbin/mfsmaster start 启动master服务
- /usr/sbin/mfscgiserv 启动服务器的cgi 浏览器端 http://master ip:9425
- /usr/sbin/mfsmetalogger start 启动备用服务
- /usr/sbin/mfschunkserver start   启动存储服务
- chown -R mfs:mfs /mnt2 给新增的文件系统赋权限
- /usr/bin/mfsmount /mnt/mfs -H mfsmaster 客户端挂载mfs系统 [加密挂载数据盘]
- 安全配置
- 挂载回收站
- ...




- 参考:  
    > * [Install PCRE Library][2]
		> * [set-up-moosefs-on-ubuntu-12-04-server][3]
		> * [Ubuntu之修改用户名和主机名][4]
		> * [MooseFS分布式文件系统介绍][5]
		> * [moosefs简单入门][6]
		> * [MFS学习总结][7]
		> * [回收站的配置 以及数据恢复 节点数据备份][8]
		> * [fuse服务 加入到开机启动][9]
		> * [单点故障解决][10]
		> * [keepalived安装][11]
		> * [心跳包检测][12]
		> * [方案1:scp+密钥认证定时推送 方案2: inotify+rsync实时同步][13]




[1]: http://moosefs.org/who-is-using-moosefs.html
[2]: http://www.cyberciti.biz/faq/debian-ubuntu-linux-install-libpcre3-dev/
[3]: https://github.com/menglifang/www.menglifang.org/blob/master/source/blog/2012-07-17-set-up-moosefs-on-ubuntu-12-04-server-amd64.md
[4]: http://blog.csdn.net/way_ping_li/article/details/9839387
[5]: http://www.heminjie.com/system/linux/3114.html
[6]: http://peiqiang.net/2014/07/28/install-moosefs.html
[7]: http://www.cnblogs.com/oubo/archive/2012/05/04/2482893.html
[8]: http://www.bubuko.com/infodetail-679326.html
[9]: http://nolinux.blog.51cto.com/4824967/1601385
[10]: http://www.rootop.org/pages/2398.html
[11]: http://www.rootop.org/pages/2102.html
[12]: http://www.linuxidc.com/Linux/2012-06/62661.htm
[13]: https://segmentfault.com/a/1190000002427568
