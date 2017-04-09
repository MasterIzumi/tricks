# 环境Ubuntu 14.04LTS

## 1 安装shadowsocks-qt5
sudo add-apt-repository ppa:hzwhuang/ss-qt5  
sudo apt-get update  
sudo apt-get install shadowsocks-qt5  

## 2 打开gui配置ss，使用购买的ss服务器

## 3 配置计算机
设置-网络-代理  
手动		127.0.0.1	1080  //这里的1080是ss服务器的本地端口  

此时，浏览器就可以正常跨越GFW了，但是终端是没法翻的,貌似是因为终端不走socks？终端不翻的话apt-get update根本没法正常更新啊。。  

或者，使用火狐的foxyproxy插件对代理进行配置，以实现较为方便的切换。

## 4 安装proxychains
sudo apt-get install proxychains  

## 5 配置proxychains
sudo vim /etc/proxychains.conf  
将最后一行的 socks4 xxxx  
改成 socks5 127.0.0.1 1080  //这里的信息也是ss服务器的本地端口  

## 6 重新打开终端，需要走代理时就加上proxychains命令，例如
sudo proxychains apt-get update  
此处可以使用 proxychains curl ip.gs查看本机地址，正常的话应该在境外。  
也可使用proxychains curl google.com看网络是否连通。  

* 若提示  
ERROR: ld.so: object 'libproxychains.so.3' from LD_PRELOAD cannot be preloaded: ignored.  
说明proxychains找不到对应的配置文件，修改路径即可。  
在终端里使用find命令  
find /usr/lib/ -name libproxychains.so.3 -print  
会打印出libproxychains.so.3的路径  
然后 sudo vim /usr/bin/proxychains  
修改里面的LD_PRELOAD参数为上面打印出来的路径即可。  

* 可能在5,6步中间需要重启一下系统。  

* 由于没有配置PAC模式，所以跨越GFW时，需要先打开ss，然后修改系统网络代理。不需要代理时，需要改回原来的配置。（摊手  





