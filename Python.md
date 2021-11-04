# Python

` *和**的内涵`

一、首先要理解带位置参数和关键字参数的函数在被调用时参数的对应关系，如图：

![img](https://pic3.zhimg.com/50/v2-0cdadbf6084bc30d990ef22e307486d8_720w.jpg?source=1940ef5c)

二、理解关于python的一些解包操作的语法糖

解包操作可以理解成删掉一个变量最外面包的那一层。

\1) 一个星号*，可以解开list的"[]" 或者tuple的"()"，举例：

```python3
x = [2, 3]

y = [1, *x, 4]
# 等价于
y = [1, 2, 3, 4]
```

\2)  两个星号**，用来解开dict的花括号 "{}"，举例：

```text
tmp = {'b': 2, 'c': 3}

y = {'a': 1, **tmp, 'd':4}
# 等价于
y = {'a': 1, 'b': 2, 'c': 3, 'd':4}
```

假如是在函数调用的时对key值的string类型的dict进行解包，会变成 k=v的形式，举例：

```text
tmp = {'b': 2, 'c': 3}

func(a=1, **tmp)
# 等价于
func(a=1, b=2, c=3)
```

三、直接把dict类型转换成list类型，会仅仅保留key值，举个例子：

```text
tmp = {'a':1, 'b': 2, 'c': 3}
assert list(tmp) == ['a', 'b', 'c']
```

## 从命令行运行Python脚本和直接在Pycharm中右键运行的区别

Pycharm:

![](/home/hp/桌面/Tips/img/2021-10-14_14-22.png)

- 在 main文件中直接运行，此时pychon文件搜索路径为：（以下均忽略第三方库）

- ```
  '/home/hp/桌面/Running/packge'
  ```

- 在bin.py文件中直接运行，此时Python文件搜索路径为：(默认增加了当前路径)

- ```
  '/home/hp/桌面/Running/packge/bin', '/home/hp/桌面/Running/packge'
  ```

  
  
  
  
  **在packge目录下命令行运行时**：```pthon main.py```

 ```
  '/home/hp/桌面/Running/packge'
 ```

 在bin目录下运行时：```python bin.py```

 ```
  '/home/hp/桌面/Running/packge/bin'
 ```

所以重点关注搜索路径（顶层目录）的变化

注意：**不要再执行文件中使用相对路径   (.  or ..)   可以在模块中使用**

在模块中导入其他模块时，必须从顶层目录一级一级的导入，不能从中途导入（除非使用相对路径）。

![](/home/hp/桌面/Tips/img/2021-10-14_14-52.png)



lilb.py:

```python
from .. import  src
```

src.py:

```
from .lib import lib
```



bin.py:

```python
import sys
sys.path.append("..")  # 此处必须加，否则无法在packge/bin目录运行，
# sys.path.append(".")  #此处加的话，则可以在packge目录下运行
import src.lib.lib
from src import src         # print(sys.path) 看看
```



