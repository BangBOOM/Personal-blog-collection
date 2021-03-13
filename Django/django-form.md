# Django 动态加载页面和使用POST加载整个页面

> 这里只放上实现的代码，原理我也不太明白，适合不了解js的人
## 动态加载页面
+ demo.html文件里的修改
```php+HTML
<script>
         $(document).ready(function () {
             $('#u109').click(function () {
                 var src=$('#src_input').val();
                 $.getJSON('{% url 'appname:url_name' %}',{'src':src},function (ret) {
                     $('#tgt_out').val(ret.tgt)
                 })
             })
         })
</script>


<input type="submit" id="u109">
<textarea id="src_input"  placeholder="输入"></textarea>
<textarea id="tgt_out" readonly='readonly'></textarea>
```
+ views.py 和 urls.py 中的应内容
```python
#views.py
def get_res(request):
    src = request.GET['src']
    tgt = src
    return JsonResponse({'tgt': tgt})

def demo(request):
    return render(request,'demo.html')


# urls.py
app_name='appname'
urlpatterns=[path('demo.html',views.index_1,name="demo"),
            path('get_res',views.get_old2new,name="url_name"),]
```
---
## POST方法重载整个页面
+ demo.html
```php+HTML
<form action="{% url 'appname:url_name' %}" method="POST">{% csrf_token %}
{% csrf_token %}
<textarea id="code" name="code">{{ res }}</textarea>
<button type="submit"name="submit" οnclick="document.formName.submit()">分析</button>
<textarea id="res" name="res">{{ res }}</textarea>
</form>
```
+ views.py 和 urls.py
```python
def demo(request):
    if request.method=="GET":
        return render(request,'demo.html')
    elif request.method=="POST":
        res=request.POST.get('code')
        return render(request, 'demo.html',{'res':res})
    return render(request, 'demo.html')


app_name="appname"
urlpatterns=[
    path('',views.demo,name="appname"),
]
```