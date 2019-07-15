## 利用Python批量修改文件名

整体框架可以用下面的代码：

```python
import os

for file_name in os.listdir('.'):
    if file_name[-2:] == 'py':
        continue
    
    subpath = './' + file_name + '/'
    count = 1
    for sub_file_name in os.listdir(subpath):
        if sub_file_name[-4:] != '.bmp':
            continue
        if count < 10:
            new_name = file_name + '_' + '0' + str(count) + '.bmp'
        else:
            new_name = file_name + '_'+ str(count) + '.bmp'
        os.rename(subpath + sub_file_name, subpath + new_name)
        count += 1
```

其中`file_name`世字符串，因此可以直接进行字符串相关的操作。

主义`os.rename`函数中，第一个参数如果用相对路径，那么默认在这个Python文件所在的目录下面，因此如果要对其他子目录中的文件进行命名，需要注意使用绝对路径。

### 参考资料

1. [知乎：Python批量修改文件名](<https://zhuanlan.zhihu.com/p/32993545>)
2. [简书：Python批量修改文件名](<https://www.jianshu.com/p/c6ad6a896b1d>)



