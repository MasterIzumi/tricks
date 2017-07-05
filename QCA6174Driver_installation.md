# Install driver for QCA6174 Wi-Fi Driver

I have an Asus Strix X99 Gaming motherboard but the wifi module doesn't work well. In fact, there are some problems with the given driver in the system, so I need to install a correct one instead.  

* Download QCA6174.zip in this repo.
* Replace the folder /lib/firmware/ath10k/QCA6174 with the correct one.
* Give every .bin file authority.
* Reload the wifi driver by using 
```
sudo rmmod ath10k_pci
sudo modprobe -v ath10k_pci

```
* Finally, reboot your computer


## Reference

[](http://tieba.baidu.com/p/4432452892?pid=86241326901&cid=0#86241326901)

> 592g网卡是QCA6174，ubuntu带的驱动有问题。  
> 下载这个文件 http://pan.baidu.com/s/1c1H5Hz6 提取码：dsp5。  
> 解压出来的QCA6174文件夹在root权限下放在/lib/firmware/ath10k/文件夹里，bin文件权限改成755。然后重载驱动，重启  
> 给bin文件加上x属性 命令是sudo chmod 755 *.bin 用以下两个命令重载网卡驱动： sudo rmmod ath10k_pci sudo modprobe -v ath10k_pci 重启  
