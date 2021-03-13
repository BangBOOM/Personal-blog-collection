## 使用vultr搭建vps，并且配置ipv6

### 步骤

+ 在vultr上注册选择服务器，首先选择地区，建议新加披

+ 接着选择系统我一直用的Centos 6 x64

+ 然后选择5$/mo

+ 下面的所有选项中只要勾选ipv6，其实没选的后面在setting中可以补充

+ 然后直接提交Deplay Now

+ 安装完成后可以看到服务器的ip,使用支持ssh的终端就可以登陆了（ssh登陆服务器）

+ 输入如下命令（输入一行然后回车）：

+ `wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh`

+ `chmod +x shadowsocks.sh`

+ `./shadowsocks.sh 2>&1 | tee shadowsocks.log`

+ 接着第一行是端口默认即可直接回车

+ 然后有输入密码，选择加密方式（加密方式那里直接输入7然后回车）

+ 最后就会提示点击任意键然后继续，这里需要等一会就可以了最后登陆的信息会显示在屏幕上

+ 并且Shadowsocks会自动加入开机自动启动，不用担心重启系统无法打开

+ 在电脑上下载shadowsocks的[客户端](https://github.com/shadowsocks/shadowsocks-windows/releases/download/4.1.7.1/Shadowsocks-4.1.7.1.zip)然后输入相关ip 端口 密码 就可以使用了



---

### 支持ipv6



+ Shadowsocks的信息存储在/etc/shadowsock.json中

+ 想让其支持ipv6使用命令`vi /etc/shadowsock.json` 然后修改第一条`"server":"0.0.0.0" `变成` "server":"::"`保存退出

+ 输入`/etc/init.d/shadowsocks restart  #接着重启服务，然后登陆的时候把电脑上的ip改成ipv6`

---

### pc热点共享ss



+ pc使用网线连接后使用shadowsocks勾选允许其他设备连接，填写代理端口

+ 打开热点，以ios举例连接后，将http代理设置成手动，ip地址注意不是pc连接以太网的网址而是无线局域网里的网址，端口填写上面的代理端口，不需要设置验证保存即可以使用

---

### 加速  [参考博客](https://medium.com/@jackme256/vultr%E6%90%AD%E5%BB%BAss%E5%8F%8A%E9%94%90%E9%80%9F%E4%BC%98%E5%8C%96%E5%8A%A0%E9%80%9F%E8%AF%A6%E7%BB%86%E6%95%99%E7%A8%8B-69763d7e2cdc)

+ Google BBR 对 SS 进行优化加速

+ `wget — no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh`

+ `chmod +x bbr.sh`

+ `./bbr.sh`

+ 输入任意键重启

+ `lsmod | grep bbr #重启后输入`

+ 出现 tcp_bbr 即说明 BBR 已经启动