# bash 使用教程
脚本执行方式：
1. 直接输入文件及其路径（绝对路径或者相对路径）
```
./haha.sh   
/home/hp/桌面/haha.sh
```
2. 也可以将脚本文件作为参数传给Shell程序，使其解释并执行脚本文件的·内容
```
bash ./haha.sh
```
3. 还可以使用source命令执行文件
```
source ./haha.sh
```
如果文件没权限执行则添加权限
```
sudo chmod +x ./haha.sh
```
**注意变量赋值等号( = )之间不能有空格**
```
n=10         #True
n = 10       #False
```
Shell是一种弱类型的编程语言。(类似Python)
