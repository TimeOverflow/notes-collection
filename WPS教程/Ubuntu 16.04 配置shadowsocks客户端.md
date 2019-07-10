## Ubuntu 16.04 配置shadowsocks客户端

### Step 1 安装shadowsocks-libev

执行以下命令：

```python
sudo apt-get install software-properties-common -y
sudo add-apt-repository ppa:max-c-lv/shadowsocks-libev -y
sudo apt-get update
sudo apt install shadowsocks-libev
```

### Step 2 编写json文件

```json
{
	"server":"服务器 IP 或是域名",
	"server_port":端口号,
	"local_address": "127.0.0.1",
	"local_port":1080,
	"password":"密码",
	"timeout":300,
	"method":"加密方式 (chacha20-ietf-poly1305 / aes-256-cfb)",
	"fast_open": false
}
```

### Step 3 终端运行shadowsocks

```shell
ss-local -c /home/xxx/shadowsocks/shadowsocks.json
```

如有必要，需`sudo`

出现下图结果就为成功

![img](https://pic4.zhimg.com/80/v2-7dd4af3c48283f8e9eb2b4548add08c3_hd.jpg)

如果出现下面的结果：

```shell
bind: Address already in use
bind() error
```

则说明本地1080端口被占用，输入命令`sudo netstst -tulpn`查看占用1080端口的进程id，用`kill <id>`命令杀死该进程即可

### Step 4 Firefox设置

设置网络代理

![img](https://zhoujianshi.github.io/articles/2018/Ubuntu%E5%AE%89%E8%A3%85%E5%92%8C%E4%BD%BF%E7%94%A8shadowsocks/1.png)

### Step 5 设置后台运行和开机自启动

运行下面命令可后台运行

```shell
nohup ss-local -c /etc/shadowsocks.json /dev/null 2>&1 &
```

如需开机 自启动（建议），可以在`/etc/rc.local`中`exit 0`前面加上一行上面的命令

### 参考

1. [Ubuntu 16.04配置Shadowsocks教程](<https://zhuanlan.zhihu.com/p/47706985>)
2. [GitHub-shadowsocks](<https://github.com/shadowsocks/shadowsocks-libev#debian--ubuntu>)
3. [Ubuntu安装和使用shadowsocks](<https://zhoujianshi.github.io/articles/2018/Ubuntu%E5%AE%89%E8%A3%85%E5%92%8C%E4%BD%BF%E7%94%A8shadowsocks/index.html>)
4. [Linux配置shadowsocks客户端](<http://www.panwiki.com/linux-shadowsocks-client/>)

