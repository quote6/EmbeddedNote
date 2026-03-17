- > 原文链接
  [STM32F103 BSP学习指南_stm32 bsp-CSDN博客](https://blog.csdn.net/h050210/article/details/146059372)
- 此博客为一份详细的STM32F103系列 BSP学习指南，涵盖以下内容：
	- BSP基础概念：BSP的作用、组成、与HAL库的关系。
	  logseq.order-list-type:: number
	- STM32F103 BSP架构：分析STM32CubeMX自动生成的BSP结构，讲解各层次的代码。
	  logseq.order-list-type:: number
	- STM32CubeIDE环境下的BSP开发：如何使用STM32CubeIDE创建和管理BSP代码。
	  logseq.order-list-type:: number
	- BSP代码的编写与移植：如何自定义BSP驱动，适配不同硬件，移植到新的板子上。
	  logseq.order-list-type:: number
	- 代码示例与实践：通过LED、按键、串口、LCD等外设的BSP开发进行实战演练。
	  logseq.order-list-type:: number
	- 推荐学习资源：包含详细教程、代码示例以及视频课程。
	  logseq.order-list-type:: number
- ## STM32F103 BSP学习指南
  background-color:: red
	- ### BSP基础概念
		- **板级支持包**（BSP, Board Support Package）是针对特定硬件平台提供的一组软件组件，包含**启动代码**、**硬件抽象层**（HAL）以及**设备驱动**等，用于在硬件和上层软件（操作系统或应用程序）之间提供支持 ([驱动、BSP、HAL三者的区别和联系_bsp驱动-CSDN博客](https://blog.csdn.net/AGood_Coder/article/details/139783878?fromshare=blogdetail&sharetype=blogdetail&sharerId=139783878&sharerefer=PC&sharesource=&sharefrom=from_link))。
		- BSP的主要作用是**屏蔽具体硬件细节**、初始化板级硬件，使上层软件可以在无需关心底层细节的情况下运行。例如BSP会提供对开发板上LED、按键等外设的封装接口，上层应用通过调用BSP提供的函数控制硬件，而这些BSP函数内部再调用HAL库来完成实际硬件操作 ([HAL vs BSP vs Drivers - Stack Overflow](https://stackoverflow.com/questions/75452204/hal-vs-bsp-vs-drivers#:~:text=Board%20Support%20Package%20,GPIO%20in%20the%20device%20hardware))。
		- HAL（硬件抽象层）提供的是对单片机片上外设的统一接口封装，如GPIO、UART、SPI等，它更加贴近寄存器操作，但屏蔽了不同MCU之间的差异，提高了代码可移植性。一般来说，BSP会包含HAL库，HAL是BSP的一部分，BSP通过调用HAL接口来访问和控制硬件 。简单来说：HAL面向MCU本身，而BSP面向具体的开发板，将HAL对芯片的控制映射到板上的实际硬件。借助BSP，上层应用只需使用BSP提供的API（例如`BSP_LED_On()`点亮LED），不需要直接操作HAL的底层细节。BSP的组成通常包括：启动代码（上电后的初始化，如时钟配置等）、HAL驱动（芯片外设驱动库）和板载设备驱动（针对特定板卡上器件的驱动)。通过BSP的抽象，上层应用获得一个与硬件无关的编程接口，便于不同平台之间移植和复用。
	- ### STM32F103 BSP架构
	  collapsed:: true
		- 使用 STM32CubeMX 为 STM32F103 生成代码后，可以看到工程被分为若干层次清晰的模块，这也体现了 BSP 的架构思想。主要的目录包括：Core、Drivers（以及可能有Middlewares）等 ([STM32CubeMX 生成的代码框架解析 - 最小化工程篇(含FreeRTOS)_.mxproject-CSDN博客)](https://xdchaoren.blog.csdn.net/article/details/137017019?fromshare=blogdetail&sharetype=blogdetail&sharerId=137017019&sharerefer=PC&sharesource=&sharefrom=from_link)。其中：
			- Core 文件夹：包含应用层核心代码。例如 `main.c`（主函数和系统初始化）、`stm32f1xx_it.c`（中断处理函数）、`stm32f1xx_hal_msp.c`（HAL MSP初始化）、`system_stm32f1xx.c`（系统时钟配置）等。这些由CubeMX根据配置自动生成，开发者的应用逻辑代码也主要在此目录中编写。MSP文件用于HAL库的底层初始化配置（如开启外设时钟、配置引脚复用等），属于将HAL驱动适配具体芯片和板子的部分。Core中的代码通常用`USER CODE BEGIN/END`标记出用户可编辑区域，以防止下一次代码生成时被覆盖。
			- Drivers 文件夹：包含ST提供的驱动库代码，分为两部分：CMSIS和HAL 。
				- CMSIS（Cortex Microcontroller Software Interface Standard）：提供处理器内核和芯片设备的底层接口，例如处理器内核的启动文件、寄存器定义、异常向量表等 。在F103工程中，CMSIS目录下有芯片厂商文件（如`startup_stm32f103xb.s`启动文件，芯片寄存器头文件等）和 Arm Cortex-M 内核的接口定义。
				  HAL 驱动：即 STM32F1xx HAL 驱动库，位于`Drivers/STM32F1xx_HAL_Driver`中，包含 `Inc` 和`Src`  子目录，提供GPIO、UART、ADC等所有片上外设的驱动实现。HAL库函数封装了对寄存器的操作，提供统一的API，比如`HAL_GPIO_WritePin`等。开发者使用HAL库可以方便地控制STM32的硬件。
				- BSP驱动：对于官方支持的开发板，Drivers目录下还可能有 BSP 子目录，例如 `Drivers/BSP/STM32F1xx_Nucleo` ([应用笔记 | 使用STM32CubeMX生成配置代码的文件结构-电子发烧友网](https://www.elecfans.com/d/2250494.html#:~:text=3))。其中包含专门针对该板卡的驱动代码（如 LED 和按键的初始化函数等）。以==Nucleo-F103RB==为例，其 BSP 提供了 LED和用户按钮的封装函数（定义在`stm32f1xx_nucleo.h/c`中），例如 `BSP_LED_Init()`, `BSP_LED_On()`，`BSP_PB_GetState()` 等。CubeMX 在“Board Selector”选择了ST官方板卡时，会自动将相应 BSP 文件添加到工程并设置好包含路径。如果工程中包含 BSP 文件夹，可以在其中看到针对开发板硬件的宏定义和函数实现，例如 LED数量、引脚定义等。在Nucleo-F103的BSP中，LED数量被定义为1 (`#define LEDn 1`)，对应的 LED2 引脚被定义在 GPIOA.5 ([stm32f1xx-nucleo-bsp/stm32f1xx_nucleo.h at main · STMicroelectronics/stm32f1xx-nucleo-bsp](https://github.com/STMicroelectronics/stm32f1xx-nucleo-bsp/blob/main/stm32f1xx_nucleo.h))；用户按钮被定义在 GPIOC.13，引脚外部中断通道也在BSP中定义。这些定义确保BSP函数知道板上有哪些LED/按键以及连接在哪里。
			- 总的来说，STM32F103 的BSP架构分层为：芯片层（CMSIS启动和核心代码）、HAL层（通用外设驱动）、BSP层（板级封装），最上层是应用层（用户编写的功能代码）。应用层调用HAL库提供的接口或通过BSP提供的更高级接口来控制硬件。HAL库本身是硬件无关的（针对STM32F1系列通用），而BSP针对具体板子进行配置。通过这种架构，代码的层次清晰：修改板子相关配置时主要调整BSP层或HAL的配置宏，上层应用代码可以大部分保持不变，从而实现软硬件解耦和代码重用。
	- ### STM32CubeIDE环境下的BSP开发
	  collapsed:: true
		- 在 STM32CubeIDE 中开发BSP代码，一般遵循**分层管理**、**模块清晰**的思路，将板子相关的代码与应用逻辑分离。使用CubeMX创建STM32F103工程时，可以选择具体的MCU或ST官方板卡：
			- 如果选择ST官方板卡（如Nucleo-F103RB），CubeMX会自动勾选相应的BSP选项并下载所需的BSP驱动，将板载资源（LED、按键等）的支持代码并入工程。生成的工程会自带BSP驱动源码，开发者可以直接调用。例如，在main函数中包含BSP头文件，然后使用`BSP_LED_Init(LED2)`初始化LED，调用`BSP_LED_On(LED2)`点亮LED等。应用层通过这些函数来与板上硬件交互，BSP内部细节已经实现好。
			- 如果使用自定义板子或CubeMX未自动生成BSP代码，则可以手动创建BSP模块。推荐的做法是在工程中新增一个专门的BSP文件夹，将板级驱动放入其中 ([手把手教你学BSP（11.4）bsp实例--基于BSP的UART通信项目_bsp怎么写-CSDN博客](https://blog.csdn.net/MHD0815/article/details/143098683?fromshare=blogdetail&sharetype=blogdetail&sharerId=143098683&sharerefer=PC&sharesource=&sharefrom=from_link))。例如，可以创建`bsp_led.h/c` , `bsp_key.h/c` 等文件，用于封装控制板上LED和按键的操作。通过这种方式，将板子的具体引脚、外设配置集中管理，主程序只需调用BSP接口。
		- 在CubeIDE中管理BSP代码的步骤可能如下：
			- **创建工程并配置外设**：使用STM32CubeIDE的集成CubeMX工具，配置好STM32F103的时钟和需要使用的外设引脚（如将某GPIO设为输出用于LED，开启USART1用于串口等）并生成代码。CubeMX会生成HAL初始化代码（如`HAL_GPIO_Init`调用等）放在 `Core/Src` 下。
			  logseq.order-list-type:: number
			- 添加BSP源文件：在工程的Core或根目录下新建一个文件夹（如“bsp”），然后新建C源文件和对应头文件，例如 `bsp_uart.c/.h` 和 `bsp_key.c/.h` 等。这样做的目的是将板级支持代码独立出来，避免直接修改CubeMX生成的HAL初始化，并且清晰地体现模块功能。
			  logseq.order-list-type:: number
			- 编写BSP代码：在BSP源文件中，包含相应的 HAL 库头文件（如`stm32f1xx_hal.h`）和自身的头文件。然后实现所需的板级函数。例如，在 `bsp_key.c` 中实现按键初始化和读取函数，在`bsp_uart.c`  中实现串口初始化和发送函数等。编写时可以调用 HAL 库提供的API实现底层功能。
			  logseq.order-list-type:: number
				- 下面是简单的示例代码说明：
					- LED驱动：假设板上有一个LED接在PA5引脚。可在`bsp_led.c`中编写:
						- 开启GPIO时钟
						  logseq.order-list-type:: number
						- 配置PA5为推挽输出模式
						  logseq.order-list-type:: number
						- 调用`HAL_GPIO_Init()`初始化引脚
						  logseq.order-list-type:: number
						- 提供`BSP_LED_On()/Off()`函数内部调用`HAL_GPIO_WritePin()`设置引脚电平。
						  logseq.order-list-type:: number
					- 例如BSP初始化LED的大致实现:
						- 开启GPIOA时钟
						- 配置PA5为推挽输出且无上拉下拉 ( [STM32F1xx_Nucleo BSP User Manual: stm32f1xx_nucleo.c Source File - STM32F103, STM32F107, STM32F151, STM32F152 Nucleo BSP Drivers Documentation](https://documentation.help/STM32F1xx_Nucleo_BSP_Drivers/stm32f1xx__nucleo_8c_source.html#:~:text=00181%20%20%20%2F,Speed%20%20%3D%20GPIO_SPEED_FREQ_HIGH%3B%2000189))
						- 然后将该引脚输出低电平关灯。
						- 这样在应用层调用`BSP_LED_Init()`时，LED硬件就准备就绪。
					- 按键驱动：假设按键接在PA0引脚，低电平按下。`BSP_Key_Init()`需要开启GPIOA时钟并配置PA0为输入模式，建议上拉以保证未按下时为高电平。然后实现`BSP_Key_Read()`，直接调用`HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0)`读取引脚电平并返回。这样应用层通过`BSP_Key_Read()`就能获取按键状态（0或1），无需关心引脚细节。
					- 串口驱动：例如对USART1进行封装。首先在`bsp_uart.c`中定义一个UART句柄（如`UART_HandleTypeDef huart1`）。在`BSP_UART_Init()`中，开启USART1外设时钟以及相关GPIO时钟；配置USART1的TX（如PA9）为复用推挽输出、RX（PA10）为浮空输入或上拉输入，然后调用`HAL_UART_Init()`完成UART初始化。接着编写发送/接收函数，例如`BSP_UART_SendString(char *str)`内部使用`HAL_UART_Transmit(&huart1, (uint8_t*)str, strlen(str), HAL_MAX_DELAY)`发送字符串。如果需要接收中断等，也可设置HAL库的回调或使用DMA等，但初学阶段可先用Polling方式读取。通过这样的封装，应用层只需调用`BSP_UART_SendString("hello")`，串口初始化和发送的细节都由BSP处理。
					- 其他外设：对于更复杂的板载外设（如LCD屏、传感器等），可以在BSP中编写相应驱动或移植厂商提供的驱动代码。比如某些STM32开发板附带LCD屏，ST的BSP已经提供了LCD初始化和读写接口（如`LCD_IO_Init()` ,`LCD_IO_WriteData()`  等）。这些函数内部通过SPI总线发送命令和数据，实现对LCD的控制。初学者可以参考官方BSP或相关教程，将这些外设的驱动也纳入BSP层。
			- 在应用层调用BSP：在main.c中包含BSP头文件，并在合适的地方初始化BSP模块 (手把手教你学BSP（11.4）bsp实例--基于BSP的UART通信项目_bsp怎么写-CSDN博客)。通常HAL库初始化（HAL_Init()）和系统时钟配置完成后，就可以调用BSP的初始化函数。例如:
			  logseq.order-list-type:: number
				- ```c
				  HAL_Init();
				  SystemClock_Config();
				  BSP_UART_Init();
				  BSP_Key_Init();
				  ```
				- 如此会初始化板载串口和按键。然后在主循环中，就可以调用`BSP_Key_Read()`监测按键状态，调用`BSP_UART_SendString()`发送串口数据等。[手把手教你学BSP（11.4）bsp实例--基于BSP的UART通信项目_bsp怎么写-CSDN博客](https://blog.csdn.net/MHD0815/article/details/143098683?fromshare=blogdetail&sharetype=blogdetail&sharerId=143098683&sharerefer=PC&sharesource=&sharefrom=from_link)展示了一个简单逻辑：如果按键按下则通过UART发送一条消息。在这个过程中，应用层并未直接使用HAL库函数，而是通过BSP提供的接口与硬件交互，实现了应用层与硬件细节的解耦。当需要更换硬件平台时，只需调整BSP层的实现，而应用逻辑基本不变。
			- 保护和维护代码：由于CubeMX可能会重新生成代码覆盖修改，确保将BSP接口的调用放在`USER CODE`标记区以内，或将主要应用代码与CubeMX生成代码隔离开（例如放入独立的.c文件）。对于我们手动添加的BSP模块文件，CubeMX一般不会改动。今后如果更改了CubeMX配置再次生成代码，注意检查`main.c`等文件中的用户代码块是否完好无损。CubeIDE提供的工程管理可以方便地在Project Explorer中添加/移除源文件，建议定期备份或使用版本管理，以便在调整BSP或HAL配置时可以快速回退。
			  logseq.order-list-type:: number
	- ### BSP代码的编写与移植
		- 自定义BSP驱动的编写需要根据硬件资源列出板上有哪些外设，以及它们连接到STM32的哪些引脚或总线。然后为每种外设编写初始化和控制函数。编写时可参考ST官方BSP或HAL库的用法，遵循**模块化**、**可移植**的思路：
		- 确定硬件资源：列出板载LED的GPIO引脚、按键的GPIO引脚和触发方式、串口连接的USART通道及引脚、是否有外部晶振、LCD接口类型等。以此决定BSP需要提供哪些模块（如`bsp_led`、`bsp_button`、`bsp_uart`、`bsp_lcd`等）。每个模块对应一组函数和必要的内部静态变量。
		- 使用宏和配置：在BSP头文件中，可以通过宏定义板载资源的数量和引脚。例如定义`#define LEDn 1`表示有1个LED，接着定义`LED_PORT[LEDn]和LED_PIN[LEDn]`数组来存储每个LED的端口和引脚号。对于按键也可类似处理。这种设计在ST官方BSP中很常见，使得支持多个LED或按键变得方便。示例：Nucleo-F103RB板载1个LED，BSP代码中定义：
			- ```C
			  #define LEDn 1  
			  GPIO_TypeDef*  LED_PORT[LEDn] = {LED2_GPIO_PORT};  
			  const uint16_t LED_PIN[LEDn]  = {LED2_PIN};  
			  ```
			- 其中`LED2_GPIO_PORT`和`LED2_PIN`被宏定义为`GPIOA`和`GPIO_PIN_5`。如果移植到新板子（例如名为MyBoard，有2个LED分别在PB0和PB1），只需修改这些宏和数组：`#define LEDn 2，LED_PORT = {GPIOB, GPIOB}，LED_PIN = {GPIO_PIN_0, GPIO_PIN_1}`，那么BSP的LED驱动函数无需改动，其通过索引即可控制不同LED。这种通过配置适配硬件的做法大大提高了BSP代码的重用性和可移植性。
		- 实现驱动函数：根据硬件类型选择HAL库中对应的API来实现功能。例如LED和GPIO相关，用HAL GPIO函数；串口用HAL UART函数；I2C外设（如EEPROM）用HAL I2C函数，等等。确保每个函数只关注一种外设操作，遵循单一职责原则。必要时将复杂操作拆分为多个小函数或步骤，以便调试和维护。
		- BSP的移植主要分两种情况：
			- **相同MCU，不同板子**：这种情况下HAL层相同（因为MCU未变），只需调整BSP代码以匹配新板硬件。具体做法包括：更新引脚宏定义；如果板载外设有增减，增删对应的驱动模块；修改初始化参数（如串口波特率、I2C地址等）以适应新硬件。由于应用代码都调用的是BSP接口，移植后应用层代码几乎无需改变。举例来说，从Nucleo-F103RB板（LED在PA5）移植到“BluePill”板（LED在PC13）：我们修改BSP的LED引脚定义为PC13，并相应更改时钟启用和初始化代码中的端口，从GPIOA改为GPIOC。完成这些修改后，调用BSP_LED_On(0)依然可以点亮LED，而不需要改动应用逻辑。关键是保证BSP提供的函数名和语义保持不变，使应用对底层变化无感知。
			- **不同系列MCU**：例如从STM32F103移植到STM32F407。这不仅板子变了，MCU架构也不同。此时需要使用新的HAL库驱动（F4系列），因此建议重新用CubeMX为目标MCU生成基础工程，然后将原BSP代码移植过去。具体步骤：首先修改BSP代码包含的头文件（比如将`stm32f1xx_hal.h`换成`stm32f4xx_hal.h`），以及对应的HAL API 调用（多数HAL函数名在不同系列中是一致的，只是前缀改变）。然后检查时钟启用宏、引脚定义等是否需要更新（如GPIO时钟宏可能从`__HAL_RCC_GPIOA_CLK_ENABLE()`变为`__HAL_RCC_GPIOA_CLK_ENABLE()`，大体相同）。再根据新板资源调整宏定义。由于不同系列的HAL库接口设计相似，大部分BSP逻辑可以复用，只需少量修改即可编译通过。移植后经过编译和简单测试，确认各BSP函数在新平台下工作正常（例如LED能亮灭，串口能收发等）。完成这些工作，应用层代码就可以在新的硬件上运行，达到快速迁移的目的。
		- 适配不同硬件平台时，还应注意：如果新板增加了一些之前没有的外设（比如增加了一个新的传感器），那么需要在BSP中新增相应驱动模块；反之如果某些外设没有了，可以移除相关代码避免冗余。BSP层的良好组织（通过宏控制、分模块文件）能让这些改动比较容易进行。总之，BSP移植的核心思路是“配置重于代码”：通过修改配置宏和初始化参数来适配新硬件，而不是推倒重写所有驱动代码。这体现了BSP的抽象优势，也是我们在编写BSP时应努力遵循的准则。正因如此，当硬件改变时，上层HAL库往往无需改动（除非换了MCU系列），我们更多关注BSP本身的调整。
	- ### 代码示例与实践
		- 通过一些具体案例，可以更直观地了解 BSP 开发流程。在此以常见的板载外设为例，演示如何使用BSP编写代码并在应用层调用它们：
			- LED控制：假设STM32F103板子上有一颗LED接在PA5引脚。我们在BSP中编写了 LED 驱动模块，提供了`BSP_LED_Init(uint8_t idx)`、`BSP_LED_On(uint8_t idx)`、`BSP_LED_Off(uint8_t idx)`、`BSP_LED_Toggle(uint8_t idx)`等函数。其实现过程是：
				- 在`BSP_LED_Init`中，使能GPIOA时钟，设置PA5为推挽输出模式，无上下拉，速度高，然后调用`HAL_GPIO_Init(GPIOA, &gpioinitstruct)`完成初始化
				- 最后写低电平熄灭LED。
				- 在`BSP_LED_On/Off`中，则简单地调用`HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET/RESET)`控制灯亮灭。
				- 在`BSP_LED_Toggle`中使用`HAL_GPIO_TogglePin`翻转引脚电平。
				- 这些函数逻辑非常直接，因为HAL已经提供了简洁的GPIO接口。使用时，应用层先调用`BSP_LED_Init(0)`初始化LED0（索引0对应PA5），然后就可以随时调用`BSP_LED_On(0)`点亮、`BSP_LED_Off(0)`熄灭或`BSP_LED_Toggle(0)`翻转状态。
				- 实践验证：可以编写一个简单的LED闪烁程序，在主循环中每隔500ms调用一次`BSP_LED_Toggle(0)`并插入`HAL_Delay(500)`，下载运行后应能看到LED周期性地闪烁。这个小实验验证了BSP LED驱动的正确性。
			- 按键读取：假设有一个用户按键连接在PC13（如Nucleo板上的USER键），低电平按下。BSP按键模块实现了`BSP_PB_Init()`和`BSP_PB_GetState()`两个函数。
				- `BSP_PB_Init`会开启GPIOC时钟，配置PC13为输入模式且上拉。若需要中断，还会配置EXTI中断优先级并开启中断线（这一部分HAL库通常在`HAL_GPIO_Init`内部根据配置自动处理）。
				- `BSP_PB_GetState()`直接返回`HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_13)`的结果——按下时返回0，松开时返回1（因为上拉）。
				- 实践验证：在主循环中调用`BSP_PB_GetState()`读取按键状态，将结果通过串口打印。如果按下按键，可以看到串口日志输出按键状态的变化。或者结合LED：可以让按键控制LED，当`BSP_PB_GetState()==0`时调用`BSP_LED_On()`，松开后调用`BSP_LED_Off()`，实现按键控制LED的效果。通过这些测试，可以确认按键BSP驱动的稳定性（需要考虑消抖，可在检测到按下后延时数十毫秒再判定）。
			- 串口通信：以USART1为例，实现BSP串口模块以收发文本信息。我们在BSP中编写了`BSP_UART_Init()`和`BSP_UART_SendString(char *str)`等函数。如前所述，Init函数配置波特率115200、8-N-1格式并调用`HAL_UART_Init`完成初始化。发送函数封装`HAL_UART_Transmit`实现同步发送。
				- 实践验证：调用`BSP_UART_Init()`后，使用PC上的串口终端连接开发板（通过USB转串口或ST-Link VCP）。在主循环中周期性地调用`BSP_UART_SendString("Hello BSP!\r\n")`发送字符串。运行程序后，应该能在终端上收到反复打印的 "Hello BSP!" 字样。这说明串口BSP驱动正常工作。
				- 接下来可以尝试通过中断接收数据：可以在HAL库的UART回调函数中调用`BSP_UART_SendString`将收到的数据回传，实现回显测试。这样一步步完善，可以做出一个简单的串口收发示例，加深对HAL UART和BSP封装的理解 。
			- LCD显示：一些开发板（如带LCD的评估板）会在BSP中包含LCD屏的驱动。例如STM32的评估板经常搭配Adafruit 1.8寸 TFT液晶屏，官方BSP提供了`BSP_LCD_Init()`、`BSP_LCD_Clear()`、`BSP_LCD_DisplayString()`等函数。这些函数内部通过调用LCD组件驱动（通常也是BSP的一部分）实现。例如`BSP_LCD_Clear()`会调用LCD低层接口将全屏像素填充为指定颜色。对于初学者，如果手头有带LCD的板子，可以参考官方提供的BSP范例。比如STM32F1的Nucleo板并不自带LCD，但其BSP代码示范了如何驱动一块插在Arduino接口上的1.8寸 SPI屏。如果要自己编写LCD的BSP驱动，可采用类似思路：先初始化SPI外设，然后根据LCD控制器的数据手册，用SPI发送初始化命令序列，接着实现写像素、写字符等高层函数。由于LCD驱动相对复杂，这里不展开，但关键是弄清硬件接口（SPI/I2C并口等）并利用HAL库完成通讯。在BSP层封装LCD操作能使主代码更简洁，例如直接调用`BSP_LCD_DisplayString("Hello")`就能在屏上显示文字，而无需关心具体的命令发送细节。
		- 综合实例：将以上外设组合，可以做一个小项目：按键控制LED并通过串口打印提示。初始化阶段调用`BSP_LED_Init()`、`BSP_PB_Init()`、`BSP_UART_Init()`准备好硬件。在主循环中监测按键：当检测到按键按下时，点亮LED并发送消息"Button Pressed!\r\n"；当按键松开时，熄灭LED并发送"Button Released!\r\n"。借助之前实现的BSP函数，这个主循环逻辑非常简单，如下所示：
			- ```C
			  while (1) {
			      if (BSP_Key_Read() == 0) {               // 按键按下
			          HAL_Delay(50);                       // 短暂延时消抖
			          if (BSP_Key_Read() == 0) {
			              BSP_LED_On(0);
			              BSP_UART_SendString("Button pressed!\r\n");
			              while (BSP_Key_Read() == 0);     // 等待按键松开
			              BSP_LED_Off(0);
			              BSP_UART_SendString("Button released!\r\n");
			          }
			      }
			  }
			  ```
			- 可以看到，应用层代码通过`BSP_Key_Read()`获取按键状态，通过`BSP_LED_On/Off()`控制LED，通过`BSP_UART_SendString()`发送串口消息。当我们更换不同板子时，只需确保BSP函数的内部实现匹配新硬件，引脚和配置正确，这段应用逻辑无需变化，就能够在新板子上运行。这正是BSP的价值所在：简化应用开发、提高代码可移植性和维护性。
	- ### 推荐学习资料
		- 想要进一步深入学习STM32F103的BSP开发，以下资源可供参考：
			- ST官方文档与示例：熟悉官方资料有助于巩固概念和获取权威信息。建议阅读 STM32CubeF1 固件库自带的《STM32F1xx HAL Driver 使用说明》、STM32F1参考手册和数据手册等文档，了解HAL底层实现细节和各外设的配置方法 ([HAL API documentation and examples - STMicroelectronics](https://community.st.com/t5/stm32-mcus-embedded-software/hal-api-documentation-and-examples/td-p/701076#:~:text=You%20can%20find%20the%20full,layer%20drivers))。同时，STM32CubeF1 包含大量示例工程，在Projects/STM32F103RB-Nucleo等目录下有针对NUCLEO-F103RB板的范例，可查看其中 BSP驱动的用法（例如 LED 和按键示例）。这些示例通常定义了USE_STM32F1xx_NUCLEO宏并调用BSP函数，可为我们提供代码参考 ([How to add HAL Library in STM32F1 in Keil. - STMicroelectronics](https://community.st.com/t5/stm32-mcus-products/how-to-add-hal-library-in-stm32f1-in-keil/td-p/132993#:~:text=Add%20some%20command%20line%20defines,Nucleo%5CExamples%5CCRC%5CCRC_Example%5CInc%5Cstm32f1xx_hal_conf.h))。通过对比官方例程和自己编写的BSP，可发现改进之处。
			- 入门教程文章：国内博客和社区有许多BSP相关的教程。例如CSDN上的《[驱动、BSP、HAL三者的区别和联系_bsp驱动-CSDN博客](https://blog.csdn.net/AGood_Coder/article/details/139783878?fromshare=blogdetail&sharetype=blogdetail&sharerId=139783878&sharerefer=PC&sharesource=&sharefrom=from_link)》一文详细解释了三者的概念及关系；“手把手教你学BSP”系列博客提供了循序渐进的实例（如GPIO控制、UART通信等），非常适合有一定嵌入式基础的开发者参考。通过阅读他人的项目实践总结，可以学到如何规划工程结构、编写通用驱动接口以及调试技巧。
			- 视频课程：观看教学视频有助于加深理解和跟随实操。例如B站上的“硬汉嵌入式 BSP驱动开发视频教程”系列涵盖了HAL库使用、非阻塞编程思想、驱动移植等高级主题，由浅入深讲解STM32板级驱动的开发 ([〖BSP视频教程〗STM32H7视频教程第1期：初识STM32H7准备工作，了解Cortex-M7内核及MDK，IAR，Embedded Studio，STM32CubeIDE和VS Code简单比较_at070tn83 v.1 stm32h7 code-CSDN博客](https://blog.csdn.net/Simon223/article/details/122512353?fromshare=blogdetail&sharetype=blogdetail&sharerId=122512353&sharerefer=PC&sharesource=&sharefrom=from_link))。该系列从GPIO、UART等基础外设开始，一步步带领观众编写BSP驱动，并提供源码下载，可跟着练习。对于偏好中文讲解的初学者，这是一个宝贵的学习资源。
			- 社区问答和文章：ST官方社区的技术文章和问答也提供了很多见解。例如《如何将BSP添加到STM32CubeIDE工程》中介绍了在CubeIDE中集成BSP库的步骤和注意事项 ([How to add a BSP to an STM32CubeIDE project - STMicroelectronics](https://community.st.com/t5/stm32-mcus/how-to-add-a-bsp-to-an-stm32cubeide-project/ta-p/49812#:~:text=STMicroelectronics%20community,and%20program%20the%20board%20STM))；还有一些经验分享帖子讨论了自定义板卡BSP驱动的设计方法 ([Board support package for STM3240G-EVAL board. - STMicroelectronics](https://community.st.com/t5/stm32-mcus-products/board-support-package-for-stm3240g-eval-board/td-p/360490#:~:text=STMicroelectronics%20community,will%20want%20either%20the))。这些内容能够帮助我们解决实际开发中遇到的问题。例如，当遇到某个外设驱动移植困难时，不妨在社区中搜索类似问题或提问，往往能得到有用的建议。
		- 开源项目参考：浏览相关的开源项目也能获益匪浅。ST官方将很多板卡的BSP代码开源在Github上，比如[stm32f1xx-nucleo-bsp仓库](https://github.com/STMicroelectronics/stm32f1xx-nucleo-bsp)，其中包含NUCLEO-F103RB的完整BSP源码。你可以研究这些代码实现，学习ST官方是如何组织BSP层、如何处理兼容性的。同时，一些通用的外设驱动项目（如FatFS文件系统移植、常用传感器驱动等）也值得参考，能拓展视野。在阅读他人代码时建议结合自己的工程实践，加以调试验证，这样印象会更深刻。
	- 以上资源提供了从基础概念、实践操作到进阶技巧的全方位学习材料。建议先从官方HAL库和简单实例入手，理解BSP与HAL的基本用法，然后循序渐进阅读博客/文档并尝试编写自己的BSP代码。通过反复实践和参考，你将逐步掌握STM32F103 BSP开发的全过程，能够更加自信地构建出稳定、可移植的嵌入式应用。祝你学习愉快！