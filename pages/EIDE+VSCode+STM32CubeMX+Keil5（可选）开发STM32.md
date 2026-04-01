- > 原文链接：[VSCode+EIDE开发STM32，支持标准库、HAL库、LL库，可以在VSCode里进行调试，编译以及下载，代码编辑更舒适，环境搭建超简单](https://www.bilibili.com/video/BV1nr4y1R7Jb/?share_source=copy_web&vd_source=fd3aa3239988b4b363737fcb2a86087e)
- **支持在VSCode里调试工程**
- **相比于Keil Assistant编译更快（Keil原生编译速度，有时候还略快（测试的一些小项目）），下载烧录也更快**
- **一些工程配置（例如烧录选项、编译器版本等）可以在VSCode里完成，无需再借助Keil**
- ## **插件/扩展下载及配置**
  background-color:: red
	- **EIDE**
	- > **下载必备工具：实用工具（setup utility）：CppCheck、OpenOCD Programmer、GNU Arm Emdedded Toolchain(stable)**
	  **配置EIDE：搜索栏输入：elf，Convert `.axf`to`.elf` for ARMCC Compiler**
	- **C/C++ Extesion Pack**
	- > **默认下载即可**
	- **Cortex Debug**
	- >   **默认下载即可**
- ## **外部支持（编译器）**
  background-color:: red
	- **ARMCC(AC5)**
	- > **在扩展EIDE上右键，选择扩展设置**
	   **之后在Armcc v5 toolchain install directory一栏中粘贴路径，例如：`F:\ProgramFile\MDK\Core\ARM\ARMCC`**
	- **ARMCLANG(AC6)**
	- > **在扩展EIDE上右键，选择扩展设置**
	   **之后在Armcc v6 toolchain install directory一栏中粘贴路径，例如：`F:\ProgramFile\MDK\Core\ARM\ARMCLANG`**
- ## **调试**
  background-color:: red
	- > **下拉选择OpenOCD，点击调试即可**
	  **调试的步骤与Keil相似**
- ## **HAL库和LL库与标准库导入工程时的区别**
  background-color:: red
  
  >  **HAL库与LL库导入工程后只需要设置烧录器即可**
  **标准库还需要在`Project Attributes`下的`Preprocessor Definitios`一项中，添加`STM32F10X_MD`**
- ## **优缺点**
  background-color:: red
- > **优点是使用ARMCC和ARMCLANG编好像译更快，体积更小**
  **缺点是这俩好像不免费（貌似也有免费的版本可用，但为了方便就使用Keil安装后集成的版本）**
- ## **典型报错**
  background-color:: red
- > **标准库：`non-ASM statement in naked function is not supported`：此类报错是编译器版本的问题**
  >
  >   **一般这个工程之前是用Keil编译过，且是AC5编译，但在EIDE里选择了AC6**
  >
  >   **解决办法：更换回AC5版本**
  >
  > **HAL库可以自由切换版本进行编译，目前没有出现报错**
- ## **免责声明**
  background-color:: red
- > **该教程仅验证了STM32F103C8T6这块板子，HAL库和标准库版本，LL库均支持调试下载，以及STM32F407VET6的HAL库**
  >
  > **对于添加新的模块到工程以及串口重定向的研究，暂时没有进行验证**
  >
  > **由于UP在接触过HAL库之后就没有再使用标准库了，所以F4系列的标准库例程就没有进行测试**
  >
  >  **有需要的小伙伴可以仿照上文提到的：HAL库和LL库与标准库导入工程时的区别，再对导入的工程进行配置**
- ## **一些小Bug，以及应对措施**
  background-color:: red
- > **使用ST-Link-V2下载器进行烧录时，若遇到下述报错（提示可能准确些），无需担心，程序可以正常烧录**
  >
  >   **（测试时烧录STM32F103C8T6不会出现这种情况，默认配置）**
  >
  > `Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>.`
  > `Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD`
  >
  > `Info : clock speed 2000 kHz`
  > `Info : STLINK V2J37S7 (API v2) VID:PID 0483:3748`
  > `Info : Target voltage: 3.278715`
  > `Info : [stm32f4x.cpu] Cortex-M4 r0p1 processor detected`
  > `Info : [stm32f4x.cpu] target has 6 breakpoints, 4 watchpoints`
  > `Info : starting gdb server for stm32f4x.cpu on 3333`
  > `Info : Listening on port 3333 for gdb connections`
  > `Info : Unable to match requested speed 2000 kHz, using 1800 kHz`
  > `Info : Unable to match requested speed 2000 kHz, using 1800 kHz`
  > `[stm32f4x.cpu] halted due to debug-request, current mode: Thread`
  > `xPSR: 0x01000000 pc: 0x080020f0 msp: 0x20000418`
  > `Info : Unable to match requested speed 8000 kHz, using 4000 kHz`
  > `Info : Unable to match requested speed 8000 kHz, using 4000 kHz`
  > `** Programming Started **`
  > `Info : device id = 0x10076413`
  > `Info : flash size = 512 KiB`
  > `** Programming Finished **`
  > `** Verify Started **`
  > `** Verified OK **`
  > `Info : Unable to match requested speed 2000 kHz, using 1800 kHz`
  > `Info : Unable to match requested speed 2000 kHz, using 1800 kHz`
  >
  > **使用ST-Link-V2（小）进行调试时，第一次可能会报错弹窗（这里忘记截图了）**
  >
  >   **此时无需担心，只需要再次编译工程，再次点调试，大多数情况下就OK了**
  >
  >   **（但是也不排除进行了上述操作，但仍然无法解决的问题，此时推荐DAP-Link或者正版ST-Link）**
- ## **结语**
  background-color:: red
- >  **`EIDE+VSCode+ARMCC(ARMCLANG)+OpenOCD+ST-Link(DAP-Link)`只是一个可选的参考版本，开发流程随个人喜好**
  >
  >   **有能力者可以完全拥抱开源开发流程，例如：`Linux+OpenOCD+DAP-Link+Makefile+GCC`**
  >
  >   **但是也请允许别人自由选择**
  >
  > **工具只是工具，不应该“喧宾夺主”，对于新手（例如我）而言，应该多把时间花在基础知识（`C语言、算法、数据手册、参考手册、主流开发IDE`）上**
  >
  > **对于新事物（这个开发流程其实有很多人在用，所以也不算太新的事物），应该秉持包容和鼓励的心态**
  >
  > **精通某一项固然重要，但是具有更强的兼容性（美称：”复合型“）也是值得推崇的（比如熟练掌握多种开发流程）**
  >
  > **学习阶段就应该多见新事物，多接受，多质疑**
  >
  > **`Every coin has two sides`**
- ## 备注
  background-color:: red
	- 新版的EIDE可能不会自动生成`launch.json`，在EIDE中选择工程名右键选择“生成调试器配置模板”然后选择对应配置后即可进行调试。
	-