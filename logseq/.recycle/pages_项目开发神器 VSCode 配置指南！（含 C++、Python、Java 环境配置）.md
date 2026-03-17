- > 原文链接 [项目开发神器 VSCode 配置指南！（含 C++、Python、Java 环境配置）](https://cloud.tencent.com/developer/article/1901050)
- 本篇文章虽然是 VSCode 挂名，但其实介绍了两款神器: VSCode 和 Vim， 这两个结合起来，开发效率蹭蹭蹭！！！
- 之前接触过 VSCode 但很少用。总感觉写 Python 不如 pycharm 香，还得安装各种插件。但最近实习中，发现在项目上有一些较为庞大的推荐架构项目，全是 c++ 代码，后期还要基于项目代码做架构开发，这就**涉及到了写 C++ 项目，而且好几个项目一块看，还要满足互相跳转** ，因为有些变量的定义可能在继承的父类项目里面。
- 于是就又接触回 VSCode, 发现还挺好用，并且插件强大，**只用 VSCode 这一款编译器，安装不同的插件就可以开发不同的项目，比如 C++， Java (Scala) 和 Python 项目，并且还免费**。这篇文章来整理下如何用 VSCode 来写这三种项目，关键是如何进行配置。因为 VSCode 本质上是一个类似于记事本的编辑器，需要各种插件和环境辅助运行。
- 大纲如下：
	- ——————————
	- ————————
	- ————————
- ## VSCode必备的基础知识
  background-color:: red
	- ### 两个必备的快捷键
	  collapsed:: true
		- 关于 VSCode 是啥，安装就是常规的安装软件操作。下载下来之后，打开，是英文版，可以打开扩展， 搜 Chinese 安装中文包， 界面如下图:
		- ![](https://ask.qcloudimg.com/http-save/yehe-4908769/b9bf74f84a1fa4f847154bab302169bd.png){:height 656, :width 1080}
		- 先记住两个快捷键:
		  collapsed:: true
			- `Ctrl + Shift + P`：这个是打开命令交互面板， 在命令面板中可以输入命令进行搜索 (中英文都可以)，然后执行。命名面板中可以执行各种命令，包括编辑器自带的功能和插件提供的功能。
			  collapsed:: true
				- ![](https://ask.qcloudimg.com/http-save/yehe-4908769/1d4b5b1de4fdd0c7d5c4fb07730d02a3.png)
			- `Ctrl + ,`：进入设置，这里可以进行用户和工作区的设置，像什么代码风格，字体风格各种设置都在这里。
			  collapsed:: true
				- ![](https://ask.qcloudimg.com/http-save/yehe-4908769/b940e2b2dd5fe8bf1dc843e4055f5835.png)
		- 这两个快捷键建议记好。
		- 另外， 这几个常用的快捷键最好也知道:
		  collapsed:: true
			- `Ctrl + Shift + E`：文件资源管理器
			- `Ctrl + Shift + F`：跨文件搜索
			- `Ctrl + Shift + G`： 源代码管理
			- `Ctrl  + Shift + D`：启动和调试
			- `Ctrl + Shift + X`：管理扩展
			- ![](https://ask.qcloudimg.com/http-save/yehe-4908769/b30174cf1c0ed043b3902f117eab3eb6.png)
	- ### 连接远程服务器开发
	  collapsed:: true
		- 这个也是需要掌握的必备技能了， 毕竟我们本地的机器啥配置自己清楚， 项目往往都放到服务器上， 而这个就保证了在自己电脑上远程打开服务器的项目并开发。
		- 这个需要安装插件 Remote-SSH， `Ctrl + Shift + X`打开安装。安装完了之后，左下角绿色的地方点击，然后选择 connect to host，输入 IP 和用户名添加即可， 这样就链接到了远程服务器。
		- ![](https://ask.qcloudimg.com/http-save/yehe-4908769/0385825c24c53a0213bf6b167e725515.png){:height 433, :width 400}
	- ### 安装Vim，使得开发更高效
	  collapsed:: true
		- 如果 vs code 上安装 vim 插件，那么写代码就可以采用 vim 的方式了， 各种便捷式命令使得开发更加高效。插件搜 vim 安装，然后点击 vim 插件，就会看到 vim 插件的安装说明。
		- ![](https://ask.qcloudimg.com/http-save/yehe-4908769/2b05cb7989e12383355c104b157dc1ca.png)
	- ### 配置Git
	  collapsed:: true
		- 这个功能我目前没用到， 因为我一般喜欢命令行直接 Git 相关操作，等具体用到了再补充。
	- ok, 几个必备知识搞定之后， 就可以开发项目了， 其它功能等用到了可以现查。
- ## VSCode 写三大编程项目的相关配置
  background-color:: red
	- ### VSCode写C++项目的配置
		- 这里记录 C++ 项目开发的相关配置，先安装 3 个插件:
			- C/C++
			- C/C++ Extension Pack
			- CodeLLDB
		- 然后检查下是否按照了 clang/clang++ 编译器
-