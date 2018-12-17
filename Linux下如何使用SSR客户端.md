#### 1、下载SSR客户端

```
git clone https://github.com/ssrbackup/shadowsocksr
```

在下载的文件中，找到 ssr 配置文件 config.json，修改以下几个参数（具体的服务器，密码，端口从SS帐号提供商那里获取）

```
"server": "0.0.0.0",    //服务器 ip 地址
"server_port":8388,        //端口
"password":"password",     //密码
 "protocol":"origin",       //协议插件
 "obfs":"http_simple",      //混淆插件
 "method":"aes-256-cfb",    //加密方式
```

运行ssr

```
python ~/shadowsocksr/shadowsocks/local.py -c /etc/shadowsocks.json
```

如果出现以下结果表示ssr客户端正常运行

```
IPv6 support
2018-04-20 15:53:35 INFO     util.py:85 loading libsodium from /home/miliimoulins/anaconda3/lib/libsodium.so.23
2018-04-20 15:53:35 INFO     local.py:50 local start with protocol[auth_sha1_v4] password [b'jfahtt'] method [chacha20] obfs [http_simple] obfs_param []
2018-04-20 15:53:35 INFO     local.py:54 starting local at 127.0.0.1:1080
2018-04-20 15:53:35 INFO     asyncdns.py:324 dns server: [('127.0.0.1', 53)]
```

如果报错请参考：
 <https://www.jianshu.com/p/a0f3268bfa33>

#### 2、转换HTTP代理

Shadowsocks默认是用 Socks5 协议的，对于Terminal 的 get，wget 等走 http 协议的地方无能为力，所以需要转换成 http 代理，加强通用性，这里使用的转换方法是基于 Polipo 的。

```
sudo apt-get install polipo      //安装Polipo
sudo gedit /etc/polipo/config   //修改配置文件
```

对 config 做如下修改：

```
# This file only needs to list configuration variables that deviate
# from the default values. See /usr/share/doc/polipo/examples/config.sample
# and "polipo -v" for variables you can tweak and further information.
logSyslog = false
logFile = "/var/log/polipo/polipo.log"

socksParentProxy = "127.0.0.1:1080"
socksProxyType = socks5

chunkHighMark = 50331648
objectHighMark = 16384

serverMaxSlots = 64
serverSlots = 16
serverSlots1 = 32

proxyAddress = "0.0.0.0"
proxyPort = 8123
```

重启Polipo:

```
/etc/init.d/polipo restart
```

验证代理是否正常工作，如果正常，就会返回抓取到的Google网页内容。此时，**终端**里面可以访问外网了。

```
export http_proxy="http://127.0.0.1:8123/"
curl www.google.com
```

#### 3、配置 Chrome 浏览器

在 Ubuntu 中 ，打开 system settings->network->network proxy，做如下修改：
 Method:Manual
 下面都设置为：127.0.0.1 端口：8123
 保存配置，此时，**Chrome** 里面可以访问外网了。

#### 4、开机后，启动 ssr 客户端

新建脚本文件：在 /home 下新建文件 shadow.sh，内容如下：

```
python ~/shadowsocksr/shadowsocks/local.py -c /etc/shadowsocks.json
```

开机后运行终端， /home 下运行 sh shadow.sh，可以正常上外网（当然终端不能关）

 

 

 

 

 