- [VSCode官网](https://code.visualstudio.com/)
- 优先推荐官方文档，先对VSCode的功能和组成有个大致的了解 [Visual Studio Code documentation](https://code.visualstudio.com/docs)
- [VSCode快捷键参考表](https://go.microsoft.com/fwlink/?linkid=832145)
-
- VSCode提供两种设置，一个是用户设置，一个是工作空间设置。用户设置在多个VSCode窗口中生效，工作空间设置只在当前的工作空间里生效，相关信息与当前的项目一起存储。同时如果进行了工作空间设置，且在用户设置里也进行了相应栏目的设置，会出现遮蔽情况。工作空间的设置会覆盖用户设置对应的设置。
-
- VSCode中一些配置文件（json后缀）
	- extensions.json：推荐当前项目使用的插件
	- package.json
	- launch.json 配置调试器
	- tasks.json 配置一系列任务，避免手动在命令行中敲重复的命令
	- settings.json 以文件夹作为工作空间打开，信息存储在该文件中，VSCode编辑和插件的相关配置
	- xxxxxx.code.snippets：项目中共享的代码片段
	-
-
- 按下`Ctrl+Shift+P`打开命令面板
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774235494737_0.png) 
	  #+END_CENTER
- 快速打开模式：在打开命令面板的情况下，删除框里的`>`，然后可以输入文件名在工作空间内搜索该文件。也可以使用`Ctrl+P`进入该模式
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774240278814_0.png) 
	  #+END_CENTER
- 符号搜索模式：在打开命令面板的情况下，将`>`替换为`#`后输入符号可以在代码文件里搜素相应的变量或函数
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774240350485_0.png) 
	  #+END_CENTER
- 按下`Ctrl+,`打开设置编辑器
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774240399338_0.png){:height 237, :width 869} 
	  #+END_CENTER
- 按下`Ctrl+Alt+F`打开搜索控件
- 按下`Ctrl+G`可以让你导航到文件中的特定行
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774240513405_0.png)
	  #+END_CENTER
-
- 按下`Ctrl+Tab`可以循环浏览最后一组打开的文件，`Alt+←`回退页面，`Alt+→`页面前进
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774240459744_0.png) 
	  #+END_CENTER
-
- 在命令面板下，输入`?`可以查看当前可以执行的命令列表
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774240542573_0.png) 
	  #+END_CENTER
- 按下`Ctrl+PageDown`切换当前编辑器右侧的编辑器
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-130624-HD.gif](../assets/QQ20260323-130624-HD_1774242400883_0.gif){:height 495, :width 724}
	  #+END_CENTER
- 按下`Ctrl+PageUp`切换到当前编辑器左侧的编辑器
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-130508-HD.gif](../assets/QQ20260323-130508-HD_1774242328199_0.gif){:height 499, :width 731}
	  #+END_CENTER
-
- `Ctrl+1`进入最左边的编辑器组
- `Ctrl+2`进入从左到右的第二个编辑器组
- `Ctrl+3`进入从左到右的第三个编辑器组
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-131229.gif](../assets/QQ20260323-131229_1774242761157_0.gif) 
	  #+END_CENTER
-
- `Ctrl+F4`关闭当前的编辑器
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-131339.gif](../assets/QQ20260323-131339_1774242844234_0.gif) 
	  #+END_CENTER
- `Ctrl+K W`关闭编辑器组中的所有编辑器
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-131828.gif](../assets/QQ20260323-131828_1774243118992_0.gif) 
	  #+END_CENTER
- `Ctrl+k Ctrl+W`关闭所有编辑器
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-132140-HD.gif](../assets/QQ20260323-132140-HD_1774243310140_0.gif) 
	  #+END_CENTER
- 在打开命令面板的情况下，输入`Interactive Editor Playground`，可以打开VSCode的交互历程，用以学习VSCode
  collapsed:: true
	- #+BEGIN_CENTER
	  ![image.png](../assets/image_1774243442929_0.png) 
	  #+END_CENTER
- ``Ctrl+` ``打开集成终端
  collapsed:: true
	- ![Integrated terminal](https://code.visualstudio.com/assets/docs/getstarted/tips-and-tricks/integrated_terminal.png){:height 404, :width 867}
- `Ctrl+B`展开/收起侧边栏
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-132824-HD.gif](../assets/QQ20260323-132824-HD_1774243720910_0.gif) 
	  #+END_CENTER
- `Ctrl+J`展开/收起面板
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-132931-HD.gif](../assets/QQ20260323-132931-HD_1774243785668_0.gif) 
	  #+END_CENTER
- `Ctrl+Shift+M`展开/收起状态栏
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-133017-HD.gif](../assets/QQ20260323-133017-HD_1774243837539_0.gif){:height 782, :width 1124} 
	  #+END_CENTER
- `Ctrl+Shift+E`显示资源管理器视图
  collapsed:: true
	- #+BEGIN_CENTER
	  ![QQ20260323-133129-HD.gif](../assets/QQ20260323-133129-HD_1774243904905_0.gif) 
	  #+END_CENTER
- `Ctrl+\`并排编辑
  collapsed:: true
	- ![QQ20260323-133258-HD.gif](../assets/QQ20260323-133258-HD_1774243988772_0.gif)
- `Ctrl+K Z`进入禅模式。在禅模式下按下两次`ESC`退出禅模式。
  collapsed:: true
	- ![QQ20260323-133551-HD.gif](../assets/QQ20260323-133551-HD_1774244166287_0.gif)
-
- 使用`Ctrl+P`进入“快速打开”界面，重复按下该快捷键可以在显示的列表中切换选择项。
  collapsed:: true
	- ![QQ20260323-141618-HD.gif](../assets/QQ20260323-141618-HD_1774246593092_0.gif)
- 同时可以在“快速打开”界面里，使用`→`按键，打开多个文件。
  collapsed:: true
	- ![QQ20260323-141925-HD.gif](../assets/QQ20260323-141925-HD_1774246781979_0.gif){:height 774, :width 1122}
-
- `Ctrl+R`打开最近的工作空间或者文件夹。
  collapsed:: true
	- ![QQ20260323-142349-HD.gif](../assets/QQ20260323-142349-HD_1774247051341_0.gif)
- `Ctrl+K F`关闭当前打开的文件夹。
-
- 使用`Alt+点击`在点击位置处添加一个光标。
  collapsed:: true
	- ![QQ20260323-142851-HD.gif](../assets/QQ20260323-142851-HD_1774247355369_0.gif)
- 使用`Ctlr+Alt+↑`和`Ctrl+Alt+↓`在当前光标位置的上方或下方创建新的光标。
  collapsed:: true
	- ![QQ20260323-143115-HD.gif](../assets/QQ20260323-143115-HD_1774247498536_0.gif)
- 使用`Ctrl+Shift+L`可以为以当前选中内容为关键词，为所有匹配项添加光标。
  collapsed:: true
	- ![QQ20260323-143434-HD.gif](../assets/QQ20260323-143434-HD_1774247692610_0.gif)
- 如果不想选中所有的匹配项，可以通过使用`Ctrl+D`来逐个为匹配项添加光标。
  collapsed:: true
	- ![QQ20260323-143707-HD.gif](../assets/QQ20260323-143707-HD_1774247844625_0.gif)
- 列选择，按住`Shift+Alt`的同时使用鼠标框选相应的文本区域，会在每个选中区域末尾添加一个独立的光标。
  collapsed:: true
	- ![tstmp_20260323144025.gif](../assets/tstmp_20260323144025_1774248026995_0.gif)
- 按住`Alt`后进行滚动可以实现快速滚动，滚动速度是默认的5倍。
  collapsed:: true
	- ![QQ20260323-144642-HD.gif](../assets/QQ20260323-144642-HD_1774248417334_0.gif)
- 在命令面板输入`View: Toggle Locked Scrollng Across Editors`命名可以使各编辑器之间同步滚动。
  collapsed:: true
	- ![tstmp_20260323145058.gif](../assets/tstmp_20260323145058_1774248660002_0.gif)
- `Shift+Alt+↑`或`Shift+Alt+↓`复制当前行内容并粘贴到当前行的前面或后面。
  collapsed:: true
	- ![QQ20260323-145608-HD.gif](../assets/QQ20260323-145608-HD_1774248981188_0.gif)
- `Alt+↑`或`Alt+↓`向上/向下移动当前行、
  collapsed:: true
	- ![QQ20260323-145842-HD.gif](../assets/QQ20260323-145842-HD_1774249132564_0.gif)
- `Shift+Alt+→`或`Shift+Alt+←`缩小或扩大选择范围。
  collapsed:: true
	- ![QQ20260323-150158-HD.gif](../assets/QQ20260323-150158-HD_1774249344470_0.gif)
- `Ctrl+Shift+O`跳转到文件中的对应的符号。
  collapsed:: true
	- ![tstmp_20260323150450.gif](../assets/tstmp_20260323150450_1774249492914_0.gif)
- 在命令面板输入`@:`可以按类别查看符号。
  collapsed:: true
	- ![group symbols by kind](https://code.visualstudio.com/assets/docs/getstarted/tips-and-tricks/group_symbols_by_kind.png)
- `Ctrl+T`跳转到工作空间中的对应符号。
- `Ctrl+U`撤销光标位置，回到上一次的光标位置。
- `Ctrl+K Ctrl+X`删除尾部空格
  collapsed:: true
	- ![QQ20260323-151234-HD.gif](../assets/QQ20260323-151234-HD_1774249973585_0.gif)
- 转换文本指令，在命令面板输入`Transform`命令，可以将选中文本转换为指定格式。
  collapsed:: true
	- ![QQ20260323-163416-HD.gif](../assets/QQ20260323-163416-HD_1774254874530_0.gif)
- `Ctrl+K Ctrl+F`当前选中行的代码进行格式操作
- `Shift+Alt+F`当前文档进行代码格式操作
- `Ctrl+Shift+[`折叠当前行代码。
- `Ctrl+Shift+]`展开当前行代码。
- `Ctrl+K Ctrl+L`对当前行代码折叠/展开。
- `Ctrl+K Ctrl+0`折叠当前编辑器里的所有代码。
- `Ctrl+ Ctrl+J`展开当前编辑器里的所有代码。
- `Ctrl+K Ctrl+/`折叠所有块注释。
- `Ctrl+L`选中当前行。
- `Ctrl+Home`跳转到当前文件开头。
- `Ctrl+End`跳转到当前文件结尾。
- `Ctrl+Shift+V`对于Markdown文件，打开文件预览。
- `Ctrl+K V`对于Markdown文件将源文件和预览窗口并排显示。
- `Ctrl+SPace`触发代码建议。
  collapsed:: true
	- ![tstmp_20260323164401.gif](../assets/tstmp_20260323164401_1774255445238_0.gif)
- 选中某个符号后按下`Alt+F12`，会在当前窗口中弹出一个小窗口显示该符号相关定义、声明和引用。
  collapsed:: true
	- ![tstmp_20260323164633.gif](../assets/tstmp_20260323164633_1774255596092_0.gif)
- `F12`或`Ctrl+点击`，选中某个符号，使用该快捷键，跳转到该符号定义处。
  collapsed:: true
	- ![tstmp_20260323164723.gif](../assets/tstmp_20260323164723_1774255646388_0.gif)
- `Shift+F12`，跳转到符号引用位置。
- `Shift+Alt+F12`选中符号的所有引用。
- `F2`，选中一个符号，对其重命名。
  collapsed:: true
	- ![tstmp_20260323165140.gif](../assets/tstmp_20260323165140_1774255902979_0.gif)
-
-
- VSCode配置代码片段 [用户自定义片段](https://code.visualstudio.com/docs/editing/userdefinedsnippets)
-
- VSCode中的任务概念
-
- VSCode断点类型
	- 条件断点
	- 触发断点
	- 内联断点
	- 函数断点
	- 数据断点
	- 日志点
-