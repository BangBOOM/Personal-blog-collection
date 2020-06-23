---
title: Django图片存取
date: 2020-04-21 21:30:29
tags:
    - django
categories:
    - web
---

解决从数据库中读取图片的地址无法打开问题。

## 修改`settings.py`文件
```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')]
        ,
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'django.template.context_processors.media',   # add this line
            ],
        },
    },
]

# 末尾添加这两行，表示路径应该在哪里，这种写法添加就在根目录自动新建media文件
MEDIA_URL='/media/' 
MEDIA_ROOT=os.path.join(BASE_DIR,'media')
```

## 修改{ProjectName}/urls.py

```
# 增加这两行导入
from django.conf import settings
from django.conf.urls.static import static

# 在urlpatterns后添加上static,用于定位到文件位置
urlpatterns = [
    url(r'^movie/',include('movies.urls'))
]+static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)
```

## 在model.py中的img字段
```
#电影海报
poster=models.ImageField(upload_to='img')
```
图片就会保存在之前的`media\img\`文件夹中