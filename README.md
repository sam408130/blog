# blog

Django + uwsgi + bootstrap + nginx 搭建自己的blog
---
[DEMO](blog.samleslie.com)


clone project

```
git clone git@github.com:sam408130/blog.git
```

创建virtualenv环境

```
cd my_blog
virtualenv pyenv
source pyenv/bin/activate
```

安装依赖

```
pip install -r requirement.txt (注意在virtualenv下，防止版本冲突)
```

本地测试
```
cd my_blog
python manage.py runserver
```
查看效果：[http://127.0.0.1:8000](http://127.0.0.1)

环境部署

```
uwsgi配置文件
[uwsgi]
# variables
projectname = my_blog #项目名称       
projectdomain = samleslie.com #域名
base = /home/dingsai/blog #项目地址       

# config
plugins = python
master = true
protocol = uwsgi
env = DJANGO_SETTINGS_MODULE=%(projectname).settings
pythonpath = %(base)/%(projectname)
module = %(projectname).wsgi
socket = 127.0.0.1:8080     
```

配置nginx

```
server {
    listen 80;
    server_name blog.samleslie.com;
    root /path/to/your/app;

    location /static/{
        alias /path/to/your/app/static/; #静态文件地址，js/css
    }
    location / {
        include uwsgi_params;
        uwsgi_pass 127.0.0.1:8080;
    }
}
```

启动uwsgi ,nginx 
```
在my_blog路径下 uwsgi uwsgi.ini

reload nginx : sudo service nginx reload

```


