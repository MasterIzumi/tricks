#解决NVIDIA显卡驱动安装后登录循环
##环境
Ubuntu 14.04 LTS  
Dell Omen 2， i7-6700HQ，  GTX960M  
##步骤
* ctrl+alt+1  
* sudo service lightdm stop  
* sudo apt-get purge nvidia*  
* sudo apt-get install nvidia-367  
