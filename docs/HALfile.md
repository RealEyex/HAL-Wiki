# HAL库文件及API简介

!> 注意：本文在文件名中出现的**ppp**表示外设的名称，比如GPIO、USART、ADC等，而且根据用户所选择的MCU型号不同，一些文件的文件名也会不同，本文参考的是STM32F3系列芯片的HAL库参考手册，因此文件开头为stm32f3

## 文件说明

### HAL相关驱动文件

#### 外设驱动文件

**stm32f3xx_hal_ppp.h**：定义了外设驱动所使用到的STM32通用的各种宏定义和数据类型（包括枚举常量、结构体、句柄）

**stm32f3xx_hal_ppp.c**：包含了外设驱动常见且主要的STM32通用API

#### 外设驱动扩展文件

**stm32f3xx_hal_ppp_ex.h**：重新定义了对于特定型号的MCU所需的宏，并且对外设的功能进行了扩展，增加了一些的特殊的结构体、枚举常量、宏定义以及函数

**stm32f3xx_hal_ppp_ex.c**：对一些特定的外设所提供的特殊的API以及重新定义的API，重新定义的API会覆盖通用API

#### HAL初始化文件

**stm32f3xx_hal.h**：包含stm32xx_hal.c文件所使用的一些结构体、枚举常量以及宏定义

**stm32f3xx_hal.c**：定义了HAL初始化或取消初始化以及相关的HAL控制API，包含了DBGMCU(调试)、Remap(重映射)、SysTick的延时函数`HAL_Delay()`

#### HAL库的官方示例

**stm32f3xx_hal_conf_template.h**：包含stm32xx_hal.c文件所使用的一些结构体、枚举常量以及宏定义

**stm32f3xx_hal_msp_template.c**：包含了各种外设的初始化以及取消初始化

#### HAL库通用的数据类型

**stm32f3xx_hal_def.h**：HAL库通用的一些结构体、枚举常量以及宏定义


### 用户应用程序文件

**system_stm32f3xx.c**

文件包含两个函数以及一个全局变量

+ SystemInit()函数：在启动文件中被调用，在进入main函数之前配置完成系统时钟
+ SystemCoreClock全局变量：保存HCLK时钟频率
+ SystemCoreClockUpdate()函数：在程序运行期间，如果修改了HCLK时钟频率，则必须调用该函数来更新`SystemCoreClock`全局变量

**startup_stm32f3xx.s**

STM32应用程序启动文件，负责完成MCU的初始化工作，包括设置SP、PC、中断向量表、配置时钟系统、初始化堆栈等工作，然后调用main函数

**stm32f3xx_hal_msp.c**

包含一些MSP(MCU Specific Package)用户回调函数，用于执行系统级别(与MCU相关的)的初始化（包括：时钟、GPIO、DMA、中断等），外设初始化通过调用`HAL_ppp_Init()`来完成，而外设所使用的硬件资源是在初始化期间调用MSP回调函数`HAL_PPP_MspInit()`所完成的，总而言之MSP用户回调函数负责在用户初始化外设前进行系统级别(与MCU相关的)的初始化。

**stm32f3xx_hal_conf.h**

HAL库的配置文件，包含HAL库各个模块文件的宏定义以之用于条件编译，实现让用户定制化的选择HAL库的模块，以便于用户自定义某个模块

**stm32f3xx_it.c/.h**

这两个文件包含异常处理程序和外设中断服务程序，其中文件中的`SysTick_Handler()`函数在Systick ISR中每`1ms`被调用一次，该函数默认会调用`HAL_IncTick()`函数，而`HAL_IncTick()`函数负责使用作应用程序时基的全局变量`uwTick`递增，且`HAL_IncTick()`函数声明为`__weak`即用户可以自己实现该函数。

**main.c/.h**

主程序文件，在该文件中除了用户代码外还应该具有以下内容：调用`HAL_Init()`函数、完成系统时钟的配置(`SystemClock_Config()`)、外设HAL驱动的初始化以及在Debug模式(调试模式)下需要实现的`assert_failed()`函数

!> 本节描述的这些文件是使用HAL构建一个应用程序所必须的最少文件。


## HAL库API简介

### 中断处理函数与回调函数

HAL外设驱动包括：

+ 形如`HAL_PPP_IRQHandler()`的外设中断处理函数
+ 以及声明为`__weak`的用户回调函数，它们通常需要用户自己重新定义

通常用户回调函数有三种：

+ 形如`HAL_PPP_MspInit()/_DeInit()`在上文提到过，在这里不再赘述
+ 形如`HAL_PPP_ProcessCpltCallback()`当某种外设完成了某个过程时触发的中断回调函数
+ 形如`HAL_PPP_ErrorCallback`当外设发生错误时触发的中断回调函数

### 通用API

通常包括以下API：

+ 初始化与取消初始化函数：`HAL_PPP_Init()`, `HAL_PPP_DeInit()`
+ I/O口操作函数：`HAL_PPP_Read()`, `HAL_PPP_Write()`, `HAL_PPP_Transmit()`, `HAL_PPP_Receive()`
+ 控制函数：`HAL_PPP_Set()`, `HAL_PPP_Get()`
+ 状态与错误函数：`HAL_PPP_GetState()`, `HAL_PPP_GetError()`

对于各种各样的外设，在上述API之外还有着功能各不相同的API。

### 扩展API

对于特定的外设所提供的，具有特殊功能的API，形如`HAL_PPPEx_XXXX()`，在外设名称后加有`Ex`字样

