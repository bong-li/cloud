# jinja2模块
### 模板语法
#### 1.语法规则
* {{ 表达式或变量名 }}
* {% 控制语句 %}
* {# 注释 #}
* 获取列表的某个索引
```
{{ list.0 }}
{{ list.1 }}
```
* 获取字典的某个键的值
```
{{ dict.key }}
```

#### 2.判断
```yaml
{% if xx %}
...
{% elif xx %}
...
{% else %}
...
{% endif %}
```

#### 3.循环
```yaml
{% for xx in xx %}
...
{% endfor %}
```

#### 4.能够传递的内容
* 字符串、数字、列表等等
* 函数（传递过去的是地址）
```python
def test(v):
    return v

with open("index.html", "r", encoding = "utf8") as fobj:
    template = jinja2.Template(fobj.read())
template.render({"func": test})
```
> index.html
```python
#通过这种方式可以调用函数
{{ func("aaa") }}
```
* 对象
***
### 基本使用
```yaml
#temp.yaml
{% if age<10 %}
child
{% elif 10<=age<18 %}
youth
{% else %}
adult
{% endif %}
```
```python
import jinja2

with open("temp.yaml", encodeing = "utf8") as fobj:
  data = fobj.read()

#创建Template的实例，传入模板的内容  
temple = jinja2.Template(data)

#传入变量的值（可以传入一个字典{'age': 15}），渲染模板
data = template.render(age = 15)

print(data)

#输出：
#youth
```
***
### 过滤器
#### 自定义过滤器
```python
import jinja2

def test(v):
    return v

env = jinja2.Environment()
env.filters["my_test"] = test

with open("index.html", "r", encoding = "utf8") as fobj:
    template = env.from_string(fobj.read())

template.render({"name": "测试"})
```
> index.html
```python
<p>{{ name|my_test }}</p>
```
> 输出结果
```
<p>测试</p>
```