- > 原文链接：[Windows平台下，以VSCode作为编辑器，搭建一套GCC编译链开发STM32。编译烧录调试代码全都可以实现，包括实时查看变量。轻量化且开源，简单易学](https://www.bilibili.com/video/BV1Hi4y1r7b3/?share_source=copy_web&vd_source=fd3aa3239988b4b363737fcb2a86087e)
- ## 实现效果
  background-color:: red
	- 可以编译代码
	  logseq.order-list-type:: number
	- 可以下载代码到开发板
	  logseq.order-list-type:: number
	- 可以设置代码优化等级
	  logseq.order-list-type:: number
	- 可以调试代码
	  logseq.order-list-type:: number
	- 可以实时查看变量值
	  logseq.order-list-type:: number
- ## 搭建开发环境所需要用到的工具/软件/硬件
  background-color:: red
	- make
	- makefile
	- xpack-openocd
	- stm32cubemx
	- st-link
	- stm32f103c8t6/stm32f407vet6
	- vscode
	- xpack-arm-gcc
	- cortex debug和C/C++这两个工具以插件的形式安装在VSCode中
- ## 前言
  background-color:: red
	- 完成本教程需要完成的前置任务：
		- 掌握**makefile**基础的语法
		- 掌握**make**工具的基础使用方法
		- 掌握**openOCD**的一些基础用法（烧录，调试）
		- 掌握**VSCode**工具的基本使用
		- 有过**Linux**的使用经验，虚拟机或物理机均可
	- 需要注意：
		- 这是一个基础向的“教学”视频，仅作为小白入门时的参考
		- 视频（文字）难免会有出错的地方，最终应用到实际请以官方为准
		- 视频（文字）教程是边学边教的实践产物，欢迎大家友好探讨，出现意见分歧较大且无法缓和时，以你为标准
		- 教程仅验证了**ST-Link**+**STM32F103C8T6/STM32F407VET6**
		- 教程仅做了裸机开发时的全流程演示
- ## 软件安装和系统环境配置
  background-color:: red
	- ### 软件安装
		- >  电脑上已经安装了**STM32CubeMX**和**VSCode**的同学可以跳过软件安装这一步
		- 安装**VSCode**，官网链接：[VSCode](https://code.visualstudio.com/download)
		- 安装**STM32CubeMX**，官网链接：[STM32CubeMX](https://www.st.com/en/development￾tools/stm32cubemx.html)
	- ### 系统环境配置
		- 需要配置**make**、**xpack-openocd**和**xpack-arm-gcc**三个工具的路径。[演示视频](https://www.bilibili.com/video/BV1Hi4y1r7b3/?
		  vd_source=6603016c0f602d079e3f53f0b71602e5)
		- [make](https://gnuwin32.sourceforge.net/packages/make.htm)
		- [xpack-openocd](https://github.com/xpack-dev-tools/openocd-xpack/releases)
		- [xpack-arm-gcc](https://github.com/xpack-dev-tools/arm-none-eabi-gcc￾xpack/releases)
	- ### 验证工具是否配置成功
		- 打开命令行分别输入以下命令
		- ```shell
		  make -v
		  openocd -v
		  arm-none-eabi-gcc -v
		  arm-none-eabi-gdb -v
		  ```
		- 按下回车后有版本提示信息说明配置成功
- ## 新建工程
  background-color:: red
	- 打开**STM32CubeMX**新建工程，最后生成代码时切换到**Make FIle**
	- 用**VSCode**打开工程，并且添加**json**脚本文件
	- 添加代码，编译代码，烧录代码，调试代码
	- 添加外部库（例如OLED，StepperMotor，MPU6050）
- ## 不足的地方
  background-color:: red
	- 串口重定向的工程未进行过验证，但鉴于这是入门教程，故不做太高要求
	- 没有配套的教程支持（例如带大家完整的用这个开发环境完成一些新项目）
	- 目前仅用这套工具链完成了LED闪烁和OLED-SPI实验，缺少一定的实践
- ## 展望和挖坑
  background-color:: red
	- 不久的将来更新**Linux**平台下的开发环境搭建教程
	- 或许之后会引入**Cmake**来进行项目管理