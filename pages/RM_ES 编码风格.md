- 首先关于RM_ES编码风格的命名是我本人（quote6）自己取的名字。风格的选择结合了ST公司的编码风格以及RT-Thread相关的编码风格。
- 编码风格可以认为一种喜好，是为了方便自己阅读。但如果大家都遵守的话，分享和学习也很方便。编码风格是一种弱要求，并不是要求必须遵守这个规则，但是更建议遵守这个规则。
- 这里我就根据自己编写代码的习惯，总结了一个自己的编码风格。风格可以通过clang-format进行格式化来实现。
- 参考链接：
	- [vscode使用clang-format格式化](https://zhuanlan.zhihu.com/p/684213308)
	- [个人学习总结--vscode中使用clang-format自定义格式化](https://zhuanlan.zhihu.com/p/1918671697499881628)
	- [clang-format最全配置注释](https://zhuanlan.zhihu.com/p/720194636)
	- [Clang-Format Style Options](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)
	- 可视化查看Clang-format设置效果 [clang-format configurator v2](https://clang-format-configurator.site/)
- ==要注意.clang-format文件的编码格式必须是utf-8编码==
- 对于来自第三方的文件，我们通常保留其原有的格式，不对其进行格式化处理，我们只保证自己编写的代码风格统一。
- ## 目录名称
	- 目录名称采用==大驼峰命名法==，多个单词之间使用`_`进行分隔，如果单词是**缩写**，则该单词的所有字母大写，例如：
		- STM32F4xx_HAL_Driver
		- DW1000
		- BSP
		- APP
		- Middlewares
		- OS
	- 因为要求并不严格，所以对于某些单词，允许全小写或大驼峰命名方式，这里给出例外的单词。
		- src
		- Src
		- Inc
		- interface
		- example
		- doc
		- test
		- MiddlewareXX（这个文件夹的命名原因与Cubemx生成有关，cubemx生成工程时可能会将Middlewares文件夹的其他文件删除，所以为了避免这种情况，就另建一个文件夹（相当于取一个类似名字的不同文件夹）来存放自己编写的代码文件，避免被cubemx更新工程时删除）
- ## 文件名称
	- 之前采取和目录一样的命名法，但之后改为如下的命名法（之前的文件名就不修改了工作量太大了）
	- 文件名采用==全小写命名==（即使单词是**缩写**），例如
		- stm32f4xx_hal_spi.c
		- rm_queue.c
	- 对于硬件相关的驱动文件命名规则是，在上面命名规则的基础上添加前缀`drv_`，例如
		- drv_led.c
		- drv_dw1000.c
		- drv_motor.c
- ## 头文件定义
	- C 语言头文件为了避免多次重复包含，需要定义一个符号。这个符号的定义形式请采用如下的风格：
	- ```c
	  #ifndef __FILE_H__
	  #define __FILE_H__
	  /* header file content */
	  #endif
	  ```
	- 即定义的符号两侧采用 "`__`" 以避免重名，另外也可以根据文件名中是否包含多个词语而采用 "`_`" 连接起来。
- ## 文件头注释
	- 在每个源文件文件头上，应该包括相应的版权信息，修改 记录：
	- ```c
	  /**
	   * @file driver_dw1000.h
	   * @author quote6 (2293127401@qq.com)
	   * @date 2025-11-24
	   * @brief 
	   * 
	   * @copyright Copyright (c) 2025 quote6 
	   * For study and research only, no reprinting
	   * 
	   * @par 修改日志
	   * <table>
	   * <tr><th>Date        <th>Version      <th>Author    <th>Description
	   * <tr><td>2025-11-24      <td>{version}   <td>quote6   <td>{Description}
	   * </table>
	   * ***************************************************************************
	   */
	  ```
	- 例如采用如上的形式。
	- 因为笔者使用的是Vscode软件并使用了“Doxygen Documentation Generator ”插件，所以只需在vscode的“setting.json”文件中添加如下内容：
	- ```yaml
	   //文件注释 版权信息模板
	      "doxdocgen.file.copyrightTag": [
	          "@copyright Copyright (c) {year} 【这里写自己的名字】 ",
	          "For study and research only, no reprinting",
	      ],
	      //文件注释，自定义模块
	      "doxdocgen.file.customTag": [
	          "@par 修改日志",
	          "<table>",
	          "<tr><th>Date        <th>Version      <th>Author    <th>Description",
	          "<tr><td>{date}      <td>{version}   <td>{author}   <td>{Description}",
	          "</table>",
	          "***************************************************************************"
	      ],
	      //文件注释的组成和顺序
	      "doxdocgen.file.fileOrder": [
	          "file",
	          "author",
	          "date",
	          "brief",
	          "empty",
	          "copyright",
	          "empty",
	          "custom"
	      ],
	      "doxdocgen.generic.authorEmail": "【这里填自己的邮箱】",
	      "doxdocgen.generic.authorName": "【这里填自己的名字】",
	      "doxdocgen.generic.authorTag": "@author {author} ({email})",
	  
	  
	      //根据自动生成的注释模板(主要用于函数注释上)
	      "doxdocgen.generic.order": [
	          "brief",
	          "param",
	          "return",
	          "custom"
	      ],
	      "doxdocgen.generic.customTags": [
	          "@note  备注",
	          "@attention  特别需要注意的地方进行说明",
	          "@warning  对函数的警告说明",
	          "@example  函数使用示例"
	      ],
	      "doxdocgen.generic.paramTemplate": "@param[in] {indent:8}{param} {indent:8} Comment",
	      "doxdocgen.generic.returnTemplate": "@return {type} ",
	      "doxdocgen.generic.splitCasingSmartText": true,
	      "doxdocgen.generic.commandSuggestionAddPrefix": true,
	  ```
- ## 自定义大驼峰命名规则
	- 这里的大驼峰命名规则是在通常的大驼峰命名规则上进行了自定义。
	- 如果名称中存在缩写单词，通常将缩写单词置于名称的前半部分，常规单词置于名称后半部分。缩写单词的全部字母大写，缩写单词之间采用“`_`”进行分隔，名称的后半部分就是常规意义上的大驼峰命名规则。如果名称只由缩写单词组成也是允许的，也就是后半部分为空。
	- 在嵌入式中，存在某些较为熟知的缩写，这些缩写可以被认为是常规单词，而不用全部大写进行区分。（尽可能满足当前规则下实施，这条规则是用于特殊情况的）
- ## 变量命名规则
  id:: 69256b1e-c8a7-4cea-9400-c4787c13350f
	- 变量名采用==小驼峰命名==规则。具体说明如下：
		- 如果变量名称只有一个单词构成（即使该单词为缩写），那么该单词的所有字母全部小写。
		- 如果变量名称不止有一个单词构成，并且没有缩写单词，那么就是常规意义上的小驼峰命名规则。
		- 如果变量名称不止有一个单词构成，但全是缩写单词，那么缩写单词之间使用“`_`”进行分隔，缩写单词的所有字母都大写，但是，对于最后一个缩写单词，将其作为普通单词看待，即该缩写单词的所有字母小写。
		- 如果变量名称不止有一个单词构成，且有缩写单词和常规单词，那么通常将缩写单词置于名称的前半部分，常规单词置于后半部分。对于前半部分，缩写单词都添加后缀“`_`”，并且其所有字母都大写，后半部分则执行常规意义上的小驼峰命名规则。
	- 如果无法将缩写单词前置，允许不改变缩写单词位置，只对其使用“`_`”进行分隔，也允许该缩写单词不大写。
	- 在嵌入式中，存在某些较为熟知的缩写，这些缩写可以被认为是常规单词，而不用全部大写进行区分。（尽可能满足当前规则下实施，这条规则是用于特殊情况的）
	- 如果变量名称过长允许进行缩写，但如果是不常见的缩写形式，要在定义的位置及时给出相应的注释，避免因为缩写影响了代码的可读性。
	- 具体举例如下：
	- ```C
	  uint16_t frameType;
	  uint8_t id;
	  uint16_t PAN_id;
	  uint8_t PAN_ID_compress;
	  void(*delay_ms)(uint32_t ms);
	  float supplyVoltage_min_v;
	  ```
- ## 常量命名规则
	- 常量通常分为三种，一种是枚举类型定义的常量，一种是宏函数定义的常量，还有一种是`const`修饰的变量。
	- 这些常量采用全部字母大写并且单词之间采用“`_`”方式进行分隔（无论是否包含缩写单词）。
	- 例如
	- ```C
	  #define USART1_PACKAGE_LEN 100
	  
	  typedef enum FrameType_e{
	      FRAME_TYPE_BEACON = 0,
	      FRAME_TYPE_DATA,
	      FRAME_TYPE_ACK,
	      FRAME_TYPE_MAC_CMD,
	      FRAME_TYPE_RESERVED,
	      FRAME_TYPE_MULTI,
	      FRAME_TYPE_FRAGMENT,
	      FRAME_TYPE_EXTENDED
	  }FrameType_t;
	  
	  const uint8_t USART1_PACK_LEN = 100;
	  ```
- ## 结构体定义
	- 原来的结构体类型命名规则采用的是==自定义大驼峰命名规则==，并添加后缀“`_s`”。
	- 关于结构体内的成员变量命名规则遵循上面的[变量命名规则](((69256b1e-c8a7-4cea-9400-c4787c13350f)))；在某些情况，由于结构体成员的名称过长，允许对某些（或全部）单词缩写，缩写后的命名规则也同上面的[变量命名规则](((69256b1e-c8a7-4cea-9400-c4787c13350f)))一样。
	- 举例如下：
	- ```C
	  typedef struct FrameControl_s{
	      uint16_t frameType : 3;    
	      uint16_t securityEnable : 1;
	      uint16_t framePending : 1;
	      uint16_t ACK_request : 1;
	      uint16_t PAN_ID_compress : 1;
	      uint16_t reserved : 3;
	      uint16_t destinationAddressMode : 2;
	      uint16_t frameVersion : 2;
	      uint16_t sourceAddressMode : 2;
	  }FrameControl_t;
	  
	  typedef struct PAN_ID_DST_s{
	      uint8_t id[DST_ADDR_LENGTH];
	  }PAN_ID_DST_t;
	  
	  typedef struct {
	      /* 省略结构体成员 */
	  }DW1000_InitTyeDef;
	  
	  typedef struct DW1000_Handle_s{
	      /* 省略结构体成员 */
	  } DW1000_Handle_t;
	  
	  typedef union MHR_802_15_4_u{
	      MHR_802_15_4_t mhr;
	      uint8_t mhrr[MAC_HEADER_LENGTH];
	  }MHR_802_15_4_ut;
	  ```
	- 结构体类型名称使用==全小写==形式，单词与单词之间采用“`_`”连接，并且添加后缀“`_s`”例如：
	- ```c
	  struct dw1000_handle_s{
	      uint8_t (*spi_init)(void);                                                         
	      uint8_t (*spi_deinit)(void);
	      uint8_t (*spi_read)(uint8_t* hdr, uint8_t hdrLen, uint8_t* buf, uint16_t bufLen);  
	      uint8_t (*spi_write)(uint8_t* hdr, uint8_t hdrLen, uint8_t* buf, uint16_t bufLen); 
	      void (*delay_ms)(uint32_t ms);
	      void (*debug_print)(const char* fmt, ...);
	      uint8_t inited;
	  };
	  ```
	- 结构体等的类型定义请以结构体名称加上 "`_t`" 的形式作为名称，例如：
	- ```c
	  typedef dw1000_handle_s dw1000_handle_t;
	  ```
- ## 枚举类型定义
	- 原来的枚举类型命名规则采用的是==自定义大驼峰命名规则==，并添加后缀“`_e`”，例如
	- ```C
	  typedef enum FrameType_e{
	      FRAME_TYPE_BEACON = 0,
	      FRAME_TYPE_DATA,
	      FRAME_TYPE_ACK,
	      FRAME_TYPE_MAC_CMD,
	      FRAME_TYPE_RESERVED,
	      FRAME_TYPE_MULTI,
	      FRAME_TYPE_FRAGMENT,
	      FRAME_TYPE_EXTENDED
	  }FrameType_t;
	  ```
	- 枚举类型名称使用==全小写==形式，单词与单词之间采用“`_`”连接，并且添加后缀“`_e`”。枚举常量的命名采用==全大写命名==，单词之间采用“`_`”进行分隔。例如：
	- ```C
	  typedef enum dest_addr_mode_e{
	      DEST_ADDR_MODE_NO_PEND_AND_ADDR = 0,
	      DEST_ADDR_MODE_RESERVED,
	      DEST_ADDR_MODE_SHORT_ADDR_16_BITS,
	      DEST_ADDR_MODE_EXT_ADDR_64_BITS
	  }dest_addr_mode_t;
	  ```
- ## 共用体类型定义
	- 原来的共用体类型命名规则采用的是==自定义大驼峰命名规则==，并添加后缀“`_u`”。
	- ```C
	  typedef union RMWCP_V2_Control_u {
	      RMWCP_V2_Control_t control;
	      uint16_t controll;  // 末尾多一个l用于跟上面的变量区分
	  }RMWCP_V2_Control_ut;
	  ```
	- 共用体类型名称使用==全小写==形式，单词与单词之间采用“`_`”连接，并且添加后缀“`_u`”。对共用体类型使用`typedef`重命名时，后缀使用“`_ut`”，以表明其具有的共用体特性。例如：
	- ```C
	  typedef union mhr_802_15_4_u{
	      MHR_802_15_4_t mhr;
	      uint8_t mhrr[MAC_HEADER_LENGTH];
	  }mhr_802_15_4_ut;
	  ```
	-
- ## 函数指针命名
	- 原来的函数指针命名规则同变量的命名规则一致。
	- ```C
	  unint8_t (*IIC_write)(uint8_t addr, uint8_t reg, uint8_t* buff, uint16_t len);
	  ```
	- 函数指针变量名称使用==全小写==形式，单词与单词之间采用“`_`”连接。对函数指针类型使用`typedef`重命名时，后缀使用“`_ft`”，以表明其具有的函数指针特性。
	- ```c
	  uint8_t (*myfunc)(uint8_t,uint8_t);
	  typedef uint8_t (*myfunc2_ft)(uint8_t,uint8_t);
	  ```
- ## 自定义类型命名
	- 自定义类型命名通常采用`typedef`进行自定义类型，通常采用==自定义大驼峰命名法==并添加后缀“`_t`”，某些情况下也会使用“`TypeDef`”。
	- 其中对于基于公用体和函数指针，其后缀名分别为`_ut`和`_ft`。
	-
- ## 宏定义
	- 使用==全大写英文名称==作为宏定义，单词之间使用 "`_`" 连接，例如：
	- ```c
	  #define DW1000_REG_DEV_ID 0x00
	  ```
	- 宏函数命名规则也类似，例如
	- ```C
	  #define DW1000_REG_BIT(bit) (1UL << bit)
	  ```
- ## 函数名称和声明
	- 之前的函数命名规则采用的==自定义大驼峰命名法==。
	- ```C
	  uint8_t LED_Init(void);
	  ```
	- 函数名称请使用小写英文的形式，单词之间使用 "`_`" 连接。提供给上层应用使用的 API接口，必须在相应的头文件中声明；如果函数入口参数是空，必须使用 `void` 作为入口参数，例如：
	- ```C
	  rt_thread_t rt_thread_self(void);
	  uint8_t dw1000_fine_grain_tx_seq_cmd(dw1000_handle_t* handle, uint16_t cmd);
	  ```
	- 内部静态函数命名：以下划线开头，使用 `_class_method` 格式，如内核或驱动文件中的函数命名：
	- ```C
	  /* IPC object init */
	  static rt_err_t _ipc_object_init();
	  /* UART driver ops */
	  static rt_err_t _uart_configure();
	  static rt_err_t _uart_control();
	  
	  static uint_t _dw1000_spi_read(dw1000_handle_t* handle, uint8_t reg, uint16_t offset, uint8_t* buf, uint16_t len);
	  ```
- ## 注释编写
	- 个人认为注释无所谓写中文还是英文，重要的是让自己能看懂。首先能让自己看懂是第一步，然后再考虑是否写得更好规范。不能失去重点。
	- ### 语句注释：
		- 个人认为无所谓注释多还是少，重要的是写的清楚明白。要记住好代码不如详细的注释。也许代码可能写得不怎么样，但是一定要把注释写清楚，因为只有写清楚了注释，才不会出现写了代码过了一段时间后就忘了这个代码是用来做什么的。同时即使自己代码写得不好，别人也能通过你的注释理解你的代码都做了什么事，并且帮你进行修改。而一旦没有注释，这些工作就会变得十分困难。所以，写注释比写代码更重要。要学会多写注释。
		- 对语句的注释只能写在它的**上方或右方**，其他位置都是非法的。
		- ```c
		  /* 你的英文注释 */
		  ```
	- ### 函数注释：
		- 注释以 `/**` 开头，以 `  */` 结尾，中间写入函数注释，组成元素如下，每个元素描述之间空一行，且首列对齐：
			- @brief + 简述函数作用。在描述中，着重说明该函数的作用，每句话首字母大写，句尾加英文句号。
			- @note + 函数说明。在上述简述中未能体现到的函数功能或作用的一些点，可以做解释说明，每句话首字母大写，句尾加英文句号。
			- @see + 相关 API 罗列。若有与当前函数相关度较高的 API，可以进行列举。
			- @param + 以参数为主语 + be 动词 + 描述，说明参数的意义或来源。
			- @return + 枚举返回值 + 返回值的意思，若返回值为数据，则直接介绍数据的功能。
			- @warning + 函数使用注意要点。在函数使用时，描述需要注意的事项，如使用环境、使用方式等。每句话首字母大写，句尾加英文句号。
		- 注释模版请参见：rt-thread/src/ipc.c 源码文件，英文注释请参考使用 grammarly 以及谷歌翻译。
			- ```C
			  /**
			  * @brief    The function will initialize a static event object.
			  *
			  * @note     For the static event object, its memory space is allocated by the compiler during compiling,
			  *           and shall placed on the read-write data segment or on the uninitialized data segment.
			  *           By contrast, the rt_event_create() function will allocate memory space automatically
			  *           and initialize the event.
			  *
			  * @see      rt_event_create()
			  *
			  * @param    event is a pointer to the event to initialize. It is assumed that storage for the event
			  *           will be allocated in your application.
			  *
			  * @param    name is a pointer to the name that given to the event.
			  *
			  * @param    value is the initial value for the event.
			  *           If want to share resources, you should initialize the value as the number of available resources.
			  *           If want to signal the occurrence of an event, you should initialize the value as 0.
			  *
			  * @param    flag is the event flag, which determines the queuing way of how multiple threads wait
			  *           when the event is not available.
			  *           The event flag can be ONE of the following values:
			  *
			  *               RT_IPC_FLAG_PRIO          The pending threads will queue in order of priority.
			  *
			  *               RT_IPC_FLAG_FIFO          The pending threads will queue in the first-in-first-out method
			  *                                         (also known as first-come-first-served (FCFS) scheduling strategy).
			  *
			  *               NOTE: RT_IPC_FLAG_FIFO is a non-real-time scheduling mode. It is strongly recommended to
			  *               use RT_IPC_FLAG_PRIO to ensure the thread is real-time UNLESS your applications concern about
			  *               the first-in-first-out principle, and you clearly understand that all threads involved in
			  *               this event will become non-real-time threads.
			  *
			  * @return   Return the operation status. When the return value is RT_EOK, the initialization is successful.
			  *           If the return value is any other values, it represents the initialization failed.
			  *
			  * @warning  This function can ONLY be called from threads.
			  */
			  rt_err_t rt_event_init(rt_event_t event, const char *name, rt_uint8_t flag)
			  {
			   ... 
			  }
			  ```
	-