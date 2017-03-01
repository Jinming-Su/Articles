## blog迁移：从csdn到emlog
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;很早就决定重新构建自己的博客，可是一直没有机会，趁着这几天还比较清闲，重新搭建了一个新的博客。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;之前一直使用的是csdn http://blog.csdn.net/u014451076. 从2014年5月到2016年10月，也有两年多的时间了。在使用过程中，一直让我诟病的主要是它的两个功能的缺陷：（１）目录功能实在是不好用，每次新增加一个目录都需要多次调整才能到达预期的位置（２）markdown编辑器语法并不标准。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这次换到了emlog http://www.sjming.net， 如果不了解emlog的可以自行官网查看。它体积很小，只有几百kb，使用php写的（所以apache+php+mysql），安装过程非常快，跟着官网介绍，五分钟就能搭建起来了。重要的是，目录管理还不错，与我之前在生产实习过程中实现的那个blog在目录实现上的方式不谋而合。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在这个新的blog上，我通常把文章在本地用.md编辑完之后，上传到github,同时使用Marxico生成一份html，上传到emlog。github上的一份是博客的备份文件。主题采用的是一个比较精简的主题，  
<br/>
![图片](https://cloud.githubusercontent.com/assets/16068384/19876503/b4e294a8-a011-11e6-998b-f09b9bd12b26.png)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;现在有几个问题如果解决了，博客就能更加完美了:
* github文章自动获取，这个见有人写过这样的插件，原理可能是直接把github上的html文件截取下来。[未解决]
* 博客文章的自动推送功能，现在可以通过手动扫每篇文章底部的二维码分享到微信朋友圈，可是每次都需要自己手动同步，如果能够自动同步就好了。[未解决]

> 正式声明一下，csdn博客http://blog.csdn.net/u014451076  从2016年10.31日正式迁移到http://www.sjming.net， csdn不再进行文章的更新。谢谢大家的支持。

<br/>
<br/>
By Sjming   
10.31 2016  
At NEU  
