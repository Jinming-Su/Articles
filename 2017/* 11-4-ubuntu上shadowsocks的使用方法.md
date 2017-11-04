### 安装
* 运行如下命令  
```
sudo apt-get install python-gevent
sudo pip install shadow socks
```

### 配置
* 配置ss，新建文件/etc/ss.json
```
{ 
  "server":"1.1.1.1",
  "server_port":1,
  "local_address":"127.0.0.1",
  "local_port":1080,
  "password":"1",
  "timeout":600,
  "method":"aes-128-ctr"
}
```
* 配置浏览器，这里选择了firefox浏览器  
先选择preferences，然后选择advanced，然后选择network，然后进行如下配置:  
![1](https://user-images.githubusercontent.com/16068384/32405788-c3bbc3a2-c1a6-11e7-82bf-f3c4171c5b8c.png)

### 运行
* 运行命令`sslocal -c /etc/ss.json`  
* 可以将上述命令使用`gnome-sessino-perperties`设置开机启动

### 注意
* shadowsocks-qt5一打开网页就崩溃
