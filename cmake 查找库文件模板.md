## cmake 查找库文件模板
```c++
# project name
PROJECT(opencv_test)
# requirement of cmake version
cmake_minimum_required(VERSION 3.5)

# set the directory of executable files
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${opencv_test_SOURCE_DIR}/bin)

# find required opencv 先找到opencv相关的文件位置（头文件和库文件两个位置）
find_package(OpenCV REQUIRED)
# directory of opencv headers将找到的头文件位置包含进去
include_directories(${OpenCV_INCLUDE_DIRS}) #(其实可以省略)
# name of executable file and path of source file
add_executable(opencv_test src/opencv_test.cpp)
# directory of opencv library 将找到的库文件位置单独列出来
link_directories(${OpenCV_LIBRARY_DIRS}) #（其实可以省略这一步）
# opencv libraries
target_link_libraries(opencv_test ${OpenCV_LIBS})
```
