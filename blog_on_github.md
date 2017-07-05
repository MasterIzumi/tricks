# Create your own Blog on GiuHub

This is a tutorial of creating a blog on github.io using Hexo.  
* Environment: Ubuntu 14.04  

## Install Nodejs

Nodejs is a js framework, we can install it by apt-get.

```
sudo apt-get install nodejs  
sudo apt-get install nodejs-legacy  
sudo apt-get install npm
```

## Hexo

### Install Hexo
```
npm install hexo-cli -g
```

### Initialization
Build a folder for blog
```
mkdir blog
```
Initialize hexo
```
hexo init ~/blog
```
Commands
```
hexo g    # generate webpage
hexo s    # run local server
```
You can test your local server by opening http://localhost:4000.

### Themes
Let's try [next]: https://github.com/iissnan/hexo-theme-next  
```
hexo clean
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
Change the theme option to next in _config.yml, and update it by:
```
cd themes/next
git pull
hexo g
hexo s
```

## Deploy the webpage on GitHub
Login your GitHub and create a new repository, the name should be "yourname.github.io".  

Install deployer:
```
npm install hexo-deployer-get --save
```

Edit _config.yml:  
```
deploy:
  type: git
  repository: https://github.com/yourname/yourname.github.io.git
  branch: master
```

Finally, you can deploy your webpage:  
```
hexo clean
hexo g
hexo d
```

* References
[http://www.jianshu.com/p/e99ed60390a8]: http://www.jianshu.com/p/e99ed60390a8  
[http://www.jianshu.com/p/a0a27d840992]: http://www.jianshu.com/p/a0a27d840992
