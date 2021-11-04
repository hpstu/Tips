# cmake

## **关于语法的疑惑**

cmake的语法还是比较灵活而且考虑到各种情况，比如

> SET(SRC_LIST main.c) 也可以写成 SET(SRC_LIST "main.c")

是没有区别的，但是假设一个源文件的文件名是fu nc.c(文件名中间包含了空格)。这时候就必须使用双引号，如果写成了SET(SRC_LIST fu nc.c)，就会出现错误，提示你找不到fu文件和nc.c文件。这种情况，就必须写成:

> SET(SRC_LIST "fu nc.c")

此外，你可以可以忽略掉source列表中的源文件后缀，比如可以写成ADD_EXECUTABLE(t1 main)，cmake会自动的在本目录查找main.c或者main.cpp等，当然，最好不要偷这个懒，以免这个目录确实存在一个main.c一个main.cpp

同时参数也可以使用分号来进行分割。下面的例子也是合法的：

> ADD_EXECUTABLE(t1 main.c t1.c)可以写成ADD_EXECUTABLE(t1 main.c;t1.c).

我们只需要在编写CMakeLists.txt时注意形成统一的风格即可。



通过外部编译进行工程构建，HELLO_SOURCE_DIR 仍然指代工程路径，即 /backup/cmake/t1 而HELLO_BINARY_DIR 则指代编译路径，即/backup/cmake/t1/build

## Examples

``` 
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo2)
```

**同级目录 指定可执行文件，库文件输出路径**
├── CMakeLists.txt
├── main.cc
├── MathFunctions.cc
└── MathFunctions.h

```cmake
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

add_library(math MathFunctions.cc)

add_executable(Demo main.cc)
target_link_libraries(Demo math)
```

输出：
├── build
│   ├── bin
│   │   └── Demo

│   ├── lib
│   │   └── libmath.a

```ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])```
这个指令用于向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存
放的位置

+ add_subdirectory(src binary_dir)           # 上级 CmakeLists.txt  但通常只需要add_subdirectory(src)   
+ ​          set(LIBRARY_OUTPUT_PATH lib)               # 下级 CmakeLists.txt

输出：

**生成的库文件会在``` build/binary/lib```**   

问题是，我应该把这两条指令写在工程的 CMakeLists.txt 还是 src 目录下的
CMakeLists.txt，把握一个简单的原则，在哪里 ADD_EXECUTABLE 或 ADD_LIBRARY，
如果需要改变目标存放路径，就在哪里加入上述的定义。



cmake ..
make
构建失败，如果需要查看细节，可以使用第一节提到的方法
**make VERBOSE=1 来构建**



为了让我们的工程能够找到 hello.h 头文件，我们需要引入一个新的指令
INCLUDE_DIRECTORIES，其完整语法为：
INCLUDE_DIRECTORIES([AFTER|BEFORE] [SYSTEM] dir1 dir2 ...)
这条指令可以用来向工程添加多个特定的头文件搜索路径，路径之间用空格分割，如果路径
中包含了空格，可以使用双引号将它括起来，默认的行为是追加到当前的头文件搜索路径的
后面，你可以通过两种方式来进行控制搜索路径添加的方式：
１，CMAKE_INCLUDE_DIRECTORIES_BEFORE，通过 SET 这个 cmake 变量为 on，可以
将添加的头文件搜索路径放在已有路径的前面。
２，通过 AFTER 或者 BEFORE 参数，也可以控制是追加还是置前。

















[`add_library` 使用指定的源文件向工程中添加一个库。](https://www.cnblogs.com/coderfenghc/archive/2012/06/23/2559603.html)

```cmake
  add_library(<name> [STATIC | SHARED | MODULE]
              [EXCLUDE_FROM_ALL]
              source1 source2 ... sourceN)
```

添加一个名为<name>的库文件，该库文件将会根据调用的命令里列出的源文件来创建。<name>对应于逻辑目标名称，而且在一个工程的全局域内必须是唯一的。待构建的库文件的实际文件名根据对应平台的命名约定来构造（比如lib<name>.a或者<name>.lib）。

`add_subdirectory` 为构建添加一个子路径。

```cmake
  add_subdirectory(source_dir [binary_dir] 
                   [EXCLUDE_FROM_ALL])
```

这条命令的作用是为构建添加一个子路径。source_dir选项指定了CMakeLists.txt源文件和代码文件的位置。如果source_dir是一个相对路径，那么source_dir选项会被解释为相对于当前的目录，但是它也可以是一个绝对路径。

binary_dir选项指定了输出文件的路径。



SET 指令的语法是：
`SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])`
现阶段，你只需要了解 SET 指令可以用来显式的定义变量即可。
比如我们用到的是 SET(SRC_LIST
main.c)，如果有多个源文件，也可以定义成：
`SET(SRC_LIST main.c t1.c t2.c)`



MESSAGE 指令的语法是：
`MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] "message to display" ...)`
这个指令用于向终端输出用户定义的信息，包含了三种类型:
SEND_ERROR，产生错误，生成过程被跳过。
SATUS ，输出前缀为 — 的信息。

FATAL_ERROR，立即终止所有 cmake 过程.



`make clean`
即可对构建结果进行清理。



指定 make install 安装文件路径：

`cmake  -D CMAKE_INSTALL_PREFIX=PATH`









