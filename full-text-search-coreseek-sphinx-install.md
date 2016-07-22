### 全文搜索coreseek 安装：
- 安装环境依赖：

  ```shell
apt-get install make gcc g++automake libtool mysql-client libmysqlclient15-dev   libxml2-dev libexpat1-dev
```

- GCC降级：

> * 先下载4.4版本的gcc、g++：
 ```shell
sudo apt-get install gcc-4.4
sudo apt-get install g++-4.4
```
 > * 然后操作如下：
 ```shell
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.4 40
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.4 40
```

- 此时可以使用 gcc -v、g++ -v 命令分别查看gcc、g++当前使用的版本已为4.4了！

- 并且重启coreseek

  ```shell
pkill -9 searchd
/usr/local/coreseek/bin/indexer --config /usr/local/coreseek/etc/sphinx.conf --all
/usr/local/coreseek/bin/searchd --config /usr/local/coreseek/etc/sphinx.conf
```
- 参考：
  > * [Ubuntu下，将GCC降级到4.4 ][1]
  > * [在Ubuntu-14.04.2上安装配置Coreseek][2]
  > * [Sphinx/Coreseek 4.1 执行 buildconf.sh 报错，无法生成configure文件 ][3]
  > * [在Mac下安装Coreseek全文搜索][4]


[1]: http://blog.csdn.net/gaojinshan/article/details/9243443

[2]: http://blog.jjyy.me/2015/07/19/ubuntu-14.04.02-install-coreseek/

[3]: http://blog.csdn.net/jcjc918/article/details/39032689

[4]: https://blog.shiniv.com/2013/08/mac-install-coreseek-full-text-search/
