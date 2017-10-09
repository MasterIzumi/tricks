# Solve "cannot open shared object file..."

## 问题描述
在运行使用g2o库的程序时，出现如下错误  
```
$ error while loading shared libraries: libg2o_core.so: cannot open shared object file: No such file or directory
```
这个错误的意思是找不到libg2o_core.so这个共享库，实际上在安装g2o时将文件放在了/usr/local/lib下。但是由于没有配置参数，系统没法到这个目录下查找，所以在运行时报错。  

## 解决办法
创建一个文件
```
$sudo vim /etc/ld.so.conf.d/g2o.conf
```
只用输入一行
```
/usr/local/lib
```
保存，退出。运行命令
```
sudo ldconfig -v
```
搞定。再运行程序时，就可以找到这个.so了。  

如果不知道在那里找.so文件，可以使用find指令，例如：
```
find /usr/ -name libg2o_core.so -print
```
如果该文件在/usr下，则会显示出其路径位置。

## Reference
* http://stackoverflow.com/questions/12335848/opencv-program-compile-error-libopencv-core-so-2-4-cannot-open-shared-object-f  
* http://www.cnblogs.com/Anker/p/3209876.html
