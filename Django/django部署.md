# Centos7下Uwsgi,Nginx部署Django

**前提已经写好了一个可用的django网站，这部分就不赘述了**

这是我的的django项目目录PersonalBlog(后面见到的PersonalBlog自行替换成自己的)

```tex
$ tree -d PersonalBlog
PersonalBlog
|-- PersonalBlog
|   `-- __pycache__
|-- __pycache__
|-- blog
|   |-- __pycache__
|   |-- migrations
|   |   `-- __pycache__
|   |-- static
|   |   |-- css
|   |   `-- pictures
|   `-- templates
|       `-- blog
`-- templates
```

---

### 安装Uwsgi，并且测试
+ `pip install uwsgi`
+ 运行一个demo测试一下，新建一个test.py文件，能成功就进行下一步。
```tex
#文件内容只要这个函数就可以了不用考虑别的
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"]
#运行命令
uwsgi --http :9090 --wsgi-file test.py
#接着打开网页输入'http://xxx:xxx:xxx:9090' 会出现hello world
#如果网页打不开应该市服务器的端口没有开，自行搜索各大云服务器如何打开端口
```
+ 接着尝试用uwsgi部署django
    + `uwsgi --chdir /xxx/PersonalBlog(一级目录) --home /xxx/anaconda3/envs/web/(你的环境如果不是虚拟环境可以不写这个) --http 127.0.0.1:9090` 
    + `--module=PersonalBlog.wsgi:application`
    + 然后浏览器输入相应网站查看网站
+ 上面这步成功后就说明uwsgi已经成功了，但是uwsgi无法加载静态文件所以就需要nginx

---
### 安装nginx，并整合
+ 在介绍nginx之前先说一下关系我们使用浏览器访问网站最先是先到nginx，然后nginx再与uwsgi通信
+ 安装nginx `yum install nginx`
+ 然后输入 `nginx` 直接再浏览器输入ip应该可以看到相应页面
+ nginx的主要作用就是访问你的静态文件，django中每个app有对应的static静态文件，而nginx只能笼统的访问一个这就需要将django中的所有static文件收集起来。
```tex
#在setting.py文件中加入这么一行
STATIC_ROOT="/home/blog/static/" #位置可以随意，这里假设这个位置方便后面
#接着运行下面的命令
python manage.py collectstatic
```
+ 然后修改 `/etc/nginx/nginx.conf` 文件,修改这个文件的目的有三个主要目的，一个是告诉nginx static文件的位置在哪里，一个是设置监听端口一般是80，还有和uwsgi通信的socket接口。
+ 在这之前我们通过uwsgi直接登陆网站跳过了socket这一步，这里将使用uwsgi的命令修改写成文件方便后续使用
```tex
#这个文件命名为xxx.ini,位置可以随便放
[uwsgi]
socket= 127.0.0.1:8000   #内部通信的端口
chdir = /xxx/PersonalBlog/
module=PersonalBlog.wsgi:application
master=True
pidfile=/xxx/PersonalBlog-master.pid
daemonize=/xxx/PersonalBlog.log #这两个是日志和进程选择一个文件单独存放
vacuum=True
max-requests=5000
harakiri=20
```
+ 使用`uwsgi --ini xxx.ini`这个命令就相当于第三步不同的是这里只用在本机上使用127.0.0.1：8000访问而没法从浏览器访问
+ 配置完了uwsgi，接着来配置 `/etc/nginx/nginx.conf`
```tex
//然后只需要修改server部分，我这里就只把server部分的内容放出来,此外把最上面改成user root;
server {
    listen       80;    //这是给浏览器访问用的
    server_name  xxxx;   //网站名字，可以写域名什么的都行
    root         /usr/share/nginx/html;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
            charset utf-8;
            include /xxx/PersonalBlog/uwsgi_params; //这个文件在/etc/nginx/下面直接复制到项目的根目录
            uwsgi_pass 127.0.0.1:8000; //这个是访问uwsgi使用的
    }
    location /static {
        root /home/blog/; //注意这里面不要重复static只需要到根目录就行了
    }
    //下面两个是出错才会有的界面不要删除
    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}
```

+ 流程到这里就结束了
+ 接下来就是运行了先把之前的进程杀干净
  + `sudo pkill -f uwsgi -9`
  + `sudo pkill -f nginx -9`
+ 启动两个服务：`uwsgi --ini /root/myfile.ini & nginx`，然后就可以访问量
+ nginx重启命令 `nginx -s reload`

---

### 最后总结一下整个流程

**其实最后就是一个`xxx.ini`文件配置uwsgi怎么启动django，修改`\etc\nginx\nginx.conf`文配置nginx**

```tex
#以我的这个为例子整个流程图
web <IP:80> nginx <-> socket <127.0.0.1:8001> uwsgi <-> django
```

---
### 参考内容

> 官方文档永远是最好的教程

+ [uwsgi document](https://uwsgi-docs-zh.readthedocs.io/zh_CN/latest/WSGIquickstart.html)
+ [How to use Django with uWSGI](https://docs.djangoproject.com/en/2.2/howto/deployment/wsgi/uwsgi/)
+ [静态文件问题csdn](https://blog.csdn.net/qq_42571805/article/details/80862455)
+ [centos下配置django、uwsgi和nginx（亲测成功）](https://blog.csdn.net/chenKFKevin/article/details/78297666)
