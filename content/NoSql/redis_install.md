Redis 官网：http://redis.io/

## 下载安装
下载最新的版本(我下载的时候是 vsn 3.0.2)

```
wget http://download.redis.io/releases/redis-3.0.2.tar.gz
tar xzf redis-3.0.2.tar.gz

LeeMac:Downloads leeyi$ mv redis-3.0.2 /usr/local/
LeeMac:Downloads leeyi$ cd /usr/local/redis-3.0.2/
LeeMac:redis-3.0.2 leeyi$ make
```
> ps: 该软件的编译安装不需要执行 ./configure 和make install 命令

如果看到有以下提示信息：
 Hint: It's a good idea to run 'make test' ;)

再执行命令:
```
make test
...
```

应该会看到如下信息：
```
 \o/ All tests passed without errors!

 Cleanup: may take some time... OK
```

> 上面的提示信息表示：测试没有错误都通过，编译成功 。

我的后续操作
```
LeeMac:redis-3.0.2 leeyi$ sudo cp src/redis-server /usr/local/bin/
Password:
LeeMac:redis-3.0.2 leeyi$ sudo cp src/redis-benchmark /usr/local/bin/
LeeMac:redis-3.0.2 leeyi$ sudo cp src/redis-cli /usr/local/bin/
LeeMac:redis-3.0.2 leeyi$ sudo cp src/redis-check-dump /usr/local/bin/
LeeMac:redis-3.0.2 leeyi$ sudo cp src/redis-check-aof /usr/local/bin/

LeeMac:redis-3.0.4 leeyi$ sudo cp src/redis-sentinel /usr/local/bin/
LeeMac:redis-3.0.4 leeyi$ sudo cp src/redis-trib.rb /usr/local/bin/
```

## 配置

复制配置文件redis.conf：
```
mkdir /usr/local/etc/redis
cp redis.conf /usr/local/etc/redis
vim /usr/local/etc/redis/redis.conf 
```

## 启动redis：
```
redis-server /usr/local/etc/redis/redis.conf 
```

控制台会看到如下信息：
```
43626:M 12 Jun 09:05:52.220 * Increased maximum number of open files to 10032 (it was originally set to 256).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 3.0.2 (87bb2945/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 43626
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

43626:M 12 Jun 09:05:52.227 # Server started, Redis version 3.0.2
43626:M 12 Jun 09:05:52.227 * The server is now ready to accept connections on port 6379
```
> ps：默认配置中redis程序启动不是以后台守护进程的模式启动的


## 配置说明
修改配置文件：/usr/local/etc/redis/redis.conf

具体的参数含义可以看conf文件中的注释，测试时只是简单修改了下面几个参数：
```
daemonize no # yes 是否后天守护进程

logfile "/Users/leeyi/workspace/web_conf/logs/redis.log" #日志文件
dir "/Users/leeyi/apps/data/redis_db/" # 目录设置
```


> ps: 要确保你设置的目录已经存在  

执行下面的测试命令：
```
edis-server /usr/local/etc/redis/redis.conf
redis-cli
127.0.0.1:6379> set mysite leeyi.net
OK

127.0.0.1:6379> get mysite
"leeyi.net"

127.0.0.1:6379> shutdown
not connected> exit
```

可以看到数据库的数据文件：
```
ls -lh
/Users/leeyi/apps/data/redis_db/
total 8
-rw-r--r--  1 leeyi  staff    38B  6 12 09:18 dump.rdb
```

可以看到redis的日志文件如下:
```
……
```

## 错误处理

### You need tcl 8.5 or newer in order to run the Redis test make: *** [test] Error 1  
```
wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz  
sudo tar xzvf tcl8.6.1-src.tar.gz  -C /usr/local/  
cd  /usr/local/tcl8.6.1/unix/  
sudo ./configure  
sudo make  
sudo make install   
```
这样以后就可以再次尝试 在 /usr/local/redis-3.0.2/ 目录下面 make test 了。
