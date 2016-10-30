# GO语言安装及vim-go的配置  
> 最近void-001在写NEUOJ_judge的后端测评机，而我本人最近也比较空闲，于是就想跟着VOID-001，看着他的代码，进行一下GO的学习。  

###GO的安装  
* 如果对自己的梯子比较有信心可以在官网下载GO的编译器，如果没信心的话，可以使用国内的golang中国http://www.golangtc.com/download（不要担心版本落后,版本是同步的），下载go1.7.3.linux-amd64.tar.gz  
* 下载好之后，先进行解压，然后就可以把的解压后文件放在自己喜欢的位置。之后进行配置：  
  
  ```  
  export GOROOT=/your path/go1.7.3.linux-amd64/go   //下载的安装包解压后的位置
  export PATH=$PATH:$GOROOT/bin    //下载的安装包解压后的位置/bin，其中是可执行文件
  export GOPATH=/like path/go  //自己喜欢的go语言的工作空间workspace，自行设置
  ```
* 到此，安装结束。使用 `go version`验证安装成功。

###GO开发环境的使用
* 很多人问GO语言使用什么开发环境，个人认为还是使用vim比较好，其实，现在主流的GO的开发环境还没有一个主流的IDE，都是需要在别的IDE上进行GO插件的配置，与其在一个几百兆的环境上开发，倒不如使用vim  
* vim 配置，Vim-go是当前使用最为广泛的用于搭建Golang开发环境的vim插件，于是我们采用vim-go插件。vim-go利 用开源Vim插件管理器安装，gmarik/Vundle.vim是目前被推荐次数更多的Vim插件管理器。可以参照官方介绍 https://github.com/VundleVim/Vundle.vim 
  * 下载。`git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`
  * 配置。在~.vimrc的开始部分，进行如下配置（直接粘贴过去即可）,
  
    ```
        " -------------  
      " Vundle  
      " https://github.com/gmarik/Vundle.vim  
      " -------------  

      set nocompatible              " be iMproved, required  
      filetype off                  " required  

      " set the runtime path to include Vundle and initialize  
      set rtp+=~/.vim/bundle/Vundle.vim  
      call vundle#begin()  
      " alternatively, pass a path where Vundle should install plugins  
      "call vundle#begin('~/some/path/here')  

      " let Vundle manage Vundle, required  
      Plugin 'gmarik/Vundle.vim'  

      " The following are examples of different formats supported.  
      " Keep Plugin commands between vundle#begin/end.  
      " plugin on GitHub repo  
      ""Plugin 'tpope/vim-fugitive'  
      " plugin from http://vim-scripts.org/vim/scripts.html  
      ""Plugin 'L9'  
      " Git plugin not hosted on GitHub  
      ""Plugin 'git://git.wincent.com/command-t.git'  
      " git repos on your local machine (i.e. when working on your own plugin)  
      ""Plugin 'file:///home/gmarik/path/to/plugin'  
      " The sparkup vim script is in a subdirectory of this repo called vim.  
      " Pass the path to set the runtimepath properly.  
      ""Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}  
      " Avoid a name conflict with L9  
      ""Plugin 'user/L9', {'name': 'newL9'}  

      " Install Vim-go  
      Plugin 'fatih/vim-go'  

      " All of your Plugins must be added before the following line  
      call vundle#end()            " required  
      filetype plugin indent on    " required  
      " To ignore plugin indent changes, instead use:  
      "filetype plugin on  
      "  
      " Brief help  
      " :PluginList       - lists configured plugins  
      " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate  
      " :PluginSearch foo - searches for foo; append `!` to refresh local cache  
      " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal  
      "  
      " see :h vundle for more details or wiki for FAQ  
      " Put your non-Plugin stuff after this line  
    ```
* 在vim中使用PluginInstall命令进行vim-go的安装，注意：在上一步的配置中，我已经了进行vim-go插件的添加。安装完成后，就可以有go语言的字符高亮显示了。
* 为了实现自动补全等功能，还需要进行GoInstallBinaries的安装。在vim中使用命令GoInstallBinaries。就会自动的安装一些二进制的可执行文件，如gocode、godef、goimports等，被安装在$GOPATH/bin下，可以ls进行命令的查看。不过这种方法一般是不行的，因为code.google.com被墙，导致一些托管在code.google.com上面的包go get不下来。不过这个不用担心，程序员的具有超能力的。下面进行解决。
  * 首先，找到我们需要安装的文件，~/.vim/bundle/vim-go/plugin/go.vim这个文件中，罗列出了需要安装的命令，其实都很小，每个只有几十KB。列举出这个的文件关键的内容。
  
    ```
        " these packages are used by vim-go and can be automatically installed if  
    " needed by the user with GoInstallBinaries  
    let s:packages = [  
                \ "github.com/nsf/gocode",   
                \ "code.google.com/p/go.tools/cmd/goimports",   
                \ "code.google.com/p/rog-go/exp/cmd/godef",   
                \ "code.google.com/p/go.tools/cmd/oracle",   
                \ "code.google.com/p/go.tools/cmd/gorename",  
                \ "github.com/golang/lint/golint",   
                \ "github.com/kisielk/errcheck",  
                \ "github.com/jstemmer/gotags",  
                \ ]  
    ```
  * 接下来，我们就需要一个文件一个文件的进行下载。这里还是在golang中国，http://www.golangtc.com/download/package，这个页面讲了下载安装的详细方法，而且非常的简单，就不说明了
* 安装好上一步的那些文件后，基本就完成了vim-go的配置，测试一下
  
  ```
  编辑hello.go：
    - 新起一行输入fmt.，然后ctrl+x, ctrl+o，Vim 会弹出补齐提示下拉框，不过并非实时跟随的那种补齐，这个补齐是由gocode提供的。
    – 输入一行代码：time.Sleep(time.Second)，执行:GoImports，Vim会自动导入time包。
    – 将光标移到Sleep函数上，执行:GoDef或命令模式下敲入gd，Vim会打开$GOROOT/src/time/sleep.go中 的Sleep函数的定义。执行:b 1返回到hellogolang.go。
    – 执行:GoLint，运行golint在当前Go源文件上。
    – 执行:GoDoc，打开当前光标对应符号的Go文档。
    – 执行:GoVet，在当前目录下运行go vet在当前Go源文件上。
    – 执行:GoRun，编译运行当前main package。
    – 执行:GoBuild，编译当前包，这取决于你的源文件，GoBuild不产生结果文件。
    – 执行:GoInstall，安装当前包。
    – 执行:GoTest，测试你当前路径下地_test.go文件。
    – 执行:GoCoverage，创建一个测试覆盖结果文件，并打开浏览器展示当前包的情况。
    – 执行:GoErrCheck，检查当前包种可能的未捕获的errors。
    – 执行:GoFiles，显示当前包对应的源文件列表。
    – 执行:GoDeps，显示当前包的依赖包列表。
    – 执行:GoImplements，显示当前类型实现的interface列表。
    – 执行:GoRename [to]，将当前光标下的符号替换为[to]。
  ```
* 实时跟随的代码补齐
