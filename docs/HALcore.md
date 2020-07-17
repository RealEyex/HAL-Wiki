# HAL内核相关函数

控制或驱动HAL内核行为的相关函数，包括`HAL_Delay()`、`HAL_IncTick()`等函数

## 初始化与取消初始化

### HAL_Init()

**原型：**`HAL_StatusTypeDef HAL_Init(void)`

**用法**

用于打开flash预取缓存以加快flash的读取，并初始化一个全局变量`uwTick`用作时基，并通过Systick产生一个以1ms为周期的中断，使`uwTick`每1ms递增一次。该函数应该在调用其它HAL库API之前调用，一般由在mian函数中变量声明定义后马上调用。

**返回值**

返回枚举常量`HAL_StatusTypeDef`，用于表示函数执行是否成功

[枚举常量`HAL_StatusTypeDef`](datatype?id=hal_statustypedef)


### HAL_DeInit()

**原型：**`HAL_StatusTypeDef HAL_DeInit(void)`

**用法**

用于取消`HAL_Init()`函数的初始化结果，并终止Systick

**返回值**

返回枚举常量`HAL_StatusTypeDef`，用于表示函数执行是否成功

[枚举常量`HAL_StatusTypeDef`](datatype?id=hal_statustypedef)


### HAL_MspInit()

**原型：**`void HAL_MspInit(void)`

**用法**

是一个用户回调函数，一般会被配置到用户文件stm32f3xx_hal_msp.c，默认行为：初始化MSP

**返回值**

该函数无返回值

### HAL_MspDeInit()

**原型：**`__weak void HAL_MspDeInit(void)`

**用法**

该函数默认是一个空函数不执行任何操作，当需要这个函数执行一些操作时应该由用户自己重新定义该函数

**返回值**

该函数无返回值


### HAL_InitTick()

**原型：**`__weak HAL_StatusTypeDef HAL_InitTick(uint32_t TickPriority)`

**用法**

该函数用来配置时基的中断优先级，该函数可以由用户根据需要重新定义

**参数**`uint32_t TickPriority`

用于时基递增中断的中断优先级

**返回值**

返回枚举常量`HAL_StatusTypeDef`，用于表示函数执行是否成功

[枚举常量`HAL_StatusTypeDef`](datatype?id=hal_statustypedef)

!> 当要在外围设备的中断处理函数内调用`HAL_Delay()`，Systick中断必须具有高于该外围设备中断的优先级


### HAL_IncTick()

**原型：**`__weak void HAL_IncTick(void)`

**用法**

该函数一般由`SysTick_Handler()`函数调用，负责使用作应用程序时基的全局变量`uwTick`递增，递增周期为1ms，该函数可以由用户根据需要重新定义

**返回值**

该函数无返回值


## 系统时钟

### HAL_Delay()

**原型：**`__weak void HAL_Delay(uint32_t Delay)`

**用法**

提供精确的毫秒级别的延时，此函数可以由用户根据需要重新定义

**参数**`uint32_t Delay`

需要延时的时间单位是毫秒(ms)

**返回值**

该函数无返回值

### HAL_SuspendTick()

**原型：**`__weak void HAL_SuspendTick(void)`

**用法**

用于禁用Systick中断，即全局变量`uwTick`的递增将会停止。此函数可以由用户根据需要重新定义

**返回值**

该函数无返回值

### HAL_ResumeTick()

**原型：**`__weak void HAL_ResumeTick(void)`

**用法**

用于重新启用Systick中断，即全局变量`uwTick`的递增将会恢复。此函数可以由用户根据需要重新定义

**返回值**

该函数无返回值

### HAL_GetTick()

**原型：**`__weak uint32_t HAL_GetTick(void)`

**用法**

用于获取全局变量`uwTick`的值，即Systick从启动到当前所经过的毫秒值。此函数可以由用户根据需要重新定义

**返回值**

Systick从启动到当前所经过的毫秒值


### HAL_GetTickPrio()

**原型：**`uint32_t HAL_GetTickPrio(void)`

**用法**

用于获取Systick中断的优先级。

**返回值**

Systick中断的优先级


### HAL_SetTickFreq()

**原型：**`HAL_StatusTypeDef HAL_SetTickFreq(HAL_TickFreqTypeDef Freq)`

**用法**

用于设置Systick的频率。

**返回值**

返回枚举常量`HAL_StatusTypeDef`，用于表示函数执行是否成功

[枚举常量`HAL_StatusTypeDef`](datatype?id=hal_statustypedef)


### HAL_GetTickFreq()

**原型：**`HAL_TickFreqTypeDef HAL_GetTickFreq(void)`

**用法**

用于获取Systick的频率。

**返回值**

返回枚举常量`HAL_TickFreqTypeDef`，用于表示函数执行是否成功

[枚举常量`HAL_TickFreqTypeDef`](datatype?id=hal_tickfreqtypedef)


## 获取HAL库或MCU版本信息

### HAL_GetHalVersion()

**原型：**`uint32_t HAL_GetHalVersion(void)`

**用法**

用于获取当前所使用的HAL库的修订版本号。

**返回值**

当前使用的HAL库的修订版本号


### HAL_GetREVID()

**原型：**`uint32_t HAL_GetREVID(void)`

**用法**

用于获取当前所使用的设备的修订版本号。

**返回值**

当前使用的设备的修订版本号


### HAL_GetDEVID()

**原型：**`uint32_t HAL_GetDEVID(void)`

**用法**

用于获取当前所使用的设备的标识符。

**返回值**

当前使用的设备的标识符


### HAL_GetUIDw0()

**原型：**`uint32_t HAL_GetUIDw0(void)`

**用法**

用于获取当前MCU的设备唯一标识符的第一个字(基于96位的UID)，需要使用自定义算法和其它两个字连接起来

**返回值**

当前MCU的设备唯一标识符的第一个字


### HAL_GetUIDw1()

**原型：**`uint32_t HAL_GetUIDw1(void)`

**用法**

用于获取当前MCU的设备唯一标识符的第二个字(基于96位的UID)，需要使用自定义算法和其它两个字连接起来

**返回值**

当前MCU的设备唯一标识符的第二个字


### HAL_GetUIDw2()

**原型：**`uint32_t HAL_GetUIDw2(void)`

**用法**

用于获取当前MCU的设备唯一标识符的第三个字(基于96位的UID)，需要使用自定义算法和其它两个字连接起来

**返回值**

当前MCU的设备唯一标识符的第三个字

## 低功耗状态下的调试模式

### HAL_DBGMCU_EnableDBGSleepMode()

**原型：**`void HAL_DBGMCU_EnableDBGSleepMode(void)`

**用法**

在睡眠模式下启用调试模块

**返回值**

该函数无返回值

!> 睡眠模式(Sleep mode)CPU时钟关闭，所有外设包括内核外设如NVIC、SysTick等仍在运行

### HAL_DBGMCU_DisableDBGSleepMode()

**原型：**`void HAL_DBGMCU_DisableDBGSleepMode(void)`

**用法**

在睡眠模式下禁用调试模块

**返回值**

该函数无返回值


### HAL_DBGMCU_EnableDBGStopMode()

**原型：**`void HAL_DBGMCU_EnableDBGStopMode(void)`

**用法**

在停机模式下启用调试模块

**返回值**

该函数无返回值

!> 停机模式(Stop mode)所有时钟都将停止

### HAL_DBGMCU_DisableDBGStopMode()

**原型：**`void HAL_DBGMCU_DisableDBGStopMode(void)`

**用法**

在停机模式下禁用调试模块

**返回值**

该函数无返回值


### HAL_DBGMCU_EnableDBGStandbyMode()

**原型：**`void HAL_DBGMCU_EnableDBGStandbyMode(void)`

**用法**

在待机模式下启用调试模块

**返回值**

该函数无返回值

!> 待机模式(Standby mode) 1.8V供电区域断电

### HAL_DBGMCU_DisableDBGStandbyMode()

**原型：**`void HAL_DBGMCU_DisableDBGStandbyMode(void)`

**用法**

在待机模式下禁用调试模块

**返回值**

该函数无返回值
