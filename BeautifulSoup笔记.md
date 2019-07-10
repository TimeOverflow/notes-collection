### 使用BeautifulSoup的一般方法

1. 配合`requests`库

   ```python
   import requests
   from bs4 import BeautifulSoup
   
   url = r'htttp://www.example.com'
   r = requests.get(url)
   """
   可能做的其他步骤
   r.encoding = r.apparent_encoding
   """
   soup = BeautifulSoup(r.text, 'html.parser')
   ```

   

2. 配合`urllib`库

   ```python
   from urllib.requests import urlopen
   from bs4 import BeautifulSoup
   
   url = r'htttp://www.example.com'
   html = urlopen(url)
   soup = BeautifulSoup(html) 
   # 或soup = BeautifulSoup(html.read())
   ```

   

### find()和find_all()函数

***Note:*** `find_all`函数是BS4中新添加的，为了符合Python的命名风格。在之前的版本中，这个函数是`findAll`，并且这个函数在BS4版本中仍然可以使用，功能与`find_all`相同，这个命名转换在其他函数中也是符合的，如`findAllNext-find_all_next`, `nextSibling-next_sibling`等。see [StackOverflow--beautifulsoup findAll find_all](https://stackoverflow.com/questions/12339323/beautifulsoup-findall-find-all)

二者的定义如下：

+ `find_all(tag, attributes, recursive=True, text, **keywords)`
+ `find(tag, attributes, recursive=True, text, **keywords)`

**返回值**：`find_all()` 方法的返回结果是值包含一个元素的列表,而 `find()` 方法直接返回结果。`find_all()` 方法没有找到目标是返回空列表, `find()` 方法找不到目标时,返回 `None` 。

`tag`可以是标签的名称或**多个标签名称组成的Python列表**，如

```python
soup.find_all("p")
soup.find_all({"h1", "h2", "h3"})
```

`attributes`是用一个Python字典封装一个标签的若干属性和对应的属性值，如

```python
soup.find_all("span", {"class": {"grenn", "red"}})
```

`keyword`参数：如果一个指定名字的参数不是搜索内置的参数名,搜索时会把该参数当作指定名字`tag`的属性来搜索。如果包含一个名字为 `id` 的参数，Beautiful Soup会搜索每个`tag`的”id”属性：

```python
soup.find_all(id='link2')
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```

如果传入 `href` 参数,Beautiful Soup会搜索每个tag的”href”属性：

```python
soup.find_all(href=re.compile("elsie"))
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
```

搜索指定名字的属性时可以使用的参数值包括 字符串 , 正则表达式 , 列表, True。

### BeautifulSoup中的对象

+ `BeautifulSoup`对象
  `soup = BeautifulSoup(html.text, 'html.parser')`

+ 标签`Tag`对象

  `soup.div.h1`,  `soup.body.p`诸如此类。

  `BeautifulSoup`对象通过`find`和`findall`函数获取的一列对象中的元素也是`Tag`对象

+ `NavigableString`对象

  标签里的文字，如html文件中有下面一行：

  ```html
  <p>
      This is a paragraph
  </p>
  ```

  那么`"This is a paragraph"`就是`NavigableString`对象的内容，通过调用`soup.p.string`可以得到这个字符串

+ `Comment`对象

  用来查找HTML文档中的注释标签，`<!-- 像这样 -->`