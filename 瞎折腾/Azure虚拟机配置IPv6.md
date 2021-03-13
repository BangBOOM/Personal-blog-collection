# 给Azure的虚拟机配置IPv6

## 扯淡

为什么要用Azure呢，因为在巨硬实习过，送的每月150$，这么大的便宜不用太浪费了。还是海外的服务器，不搭建个梯子实在太浪费了。不过Azure的各种配置实在反人类，最好用的配置方式是基于ARM（Azure Resource Template）但是那一堆东西学起来太难受了，又不是做运维，在控制面板操作显示然最方便的，问题来了面板里配置不支持直接支持IPv6。

为什么要有IPv6？ 因为校园网的IPv6流量不收费，不然游戏Netflix这种烧流浪大户，花出去的流量费用比游戏费用会员费还贵设受得了。

好了扯淡的话结束了，下面介绍具体的操作。

## 流程简述

1. 手动创建支持IPv6的虚拟网络
2. 创建虚拟机选择预先创建的虚拟网络，选择不需要IP
3. 创建网络接口，注意设置支持IPv6
4. 创建公共IP绑定到接口
5. 虚拟机关机附加新的网络接口，解除之前不支持IPv6的网络接口

## 具体步骤

### 新建资源组

随便取个名字，后面所有的内容都会放到这个资源组。

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313135713.png)

### 创建虚拟网络

IPv6地址范围自己随便设置，符合地址空间就行了，可以直接使用`ace:cab:deca:deed::/64`

![image-20210313141759947](C:\Users\WentworthY\AppData\Roaming\Typora\typora-user-images\image-20210313141759947.png)

### 创建虚拟机

主要是网络部分的选择，其余的都是正常操作，选择上一步创建的虚拟网络，公网IP选择无，其余的操作完成后直接创建。

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313142429.png)

### 配置虚拟机与网络接口

其实这个时候虚拟机还是会自动创建一个网络接口配置到虚拟网络，所以要分离这个自动新建的网络接口。

1. 关闭虚拟机
2. 进入虚拟机的网络部分，先附加新的网络接口再分离原来的网络接口（因为不能完全没有网络接口）
3. 点击附加网络接口，直接新建一个流程如下

### 创建网络接口

注意要选中专用IP地址（IPv6）

子网部分没有IPv6需要自己按照我这个设置

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313135949.png)

这个时候只是构建了接口还没有分配IP

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313140230.png)

### 创建公共IP地址

IP版本选择Both，IP名称随便填就可以了

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313140454.png)

### 绑定IP到网络接口

进入之前创建的网络接口，进到IP配置一栏

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313140737.png)

选择关联，然后在下一行的下拉框选择之前创建的demoipv4，重复这个操作配置IPv6

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313140849.png)

关联成功后会得到如下的界面

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313141208.png)

至此拥有IPv6的网络接口就配置完成了

### 启动虚拟机

至此就可以看到虚拟机里面显示有公共的IPv6了

![](https://figure-bed-y.oss-cn-beijing.aliyuncs.com/img/20210313143917.png)

