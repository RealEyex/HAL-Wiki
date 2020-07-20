# TIM(计时器)相关函数

对于STM32一般有着多种计时器，包括：高级计时器、通用计时器、基本计时器、看门狗计时器，而这些计时器都几乎拥有差不多的工作模式。

计时器工作模式：
+ 基本计时/时基(Base)模式
+ 输出比较(OC)模式
+ 输入捕获(IC)模式
+ 输出PWM(PWM)模式
+ 单脉冲(OnePulse)模式
+ 编码器(Encoder)模式

对于这些不同的工作模式HAL库提供了对于这些模式都具有的类似的函数，这些函数的对外接口基本相同，在这里将他们概括起来，称之为“基本函数”，用`XXX`来表示不同的工作模式(如：OC、IC、PWM等)

> 对于TIM在部分资料有被称作定时器，但在本文将尽量将其称作计时器或直接称为TIM

## 基本函数

### HAL_TIM_XXX_Init

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_Init(TIM_HandleTypeDef *htim)`

**用法**

用`TIM_HandleTypeDef *htim`所指向结构体的配置去初始化一个计时器，并返回该过程执行的结果

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要初始化的计时器的句柄

**返回值**

返回该函数执行的结果

### HAL_TIM_XXX_DeInit

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_DeInit(TIM_HandleTypeDef *htim)`

**用法**

对`TIM_HandleTypeDef *htim`所指向的计时器进行取消初始化，并返回该过程执行的结果

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要取消初始化的计时器的句柄

**返回值**

返回该函数执行的结果


### HAL_TIM_XXX_MspInit

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_MspInit(TIM_HandleTypeDef *htim)`

**用法**

用`TIM_HandleTypeDef *htim`所指向结构体的配置去初始化一个计时器与MCU相关的配置，并返回该过程执行的结果

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要初始化的计时器的句柄

**返回值**

返回该函数执行的结果

### HAL_TIM_XXX_MspDeInit

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_MspDeInit(TIM_HandleTypeDef *htim)`

**用法**

对`TIM_HandleTypeDef *htim`所指向的计时器进行取消与MCU相关的初始化，并返回该过程执行的结果

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要取消初始化的计时器的句柄

**返回值**

返回该函数执行的结果


### HAL_TIM_XXX_Start

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_Start(TIM_HandleTypeDef *htim, uint32_t Channel)`

**用法**

根据具体的工作模式，启动计时器进行该方面的进程

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要启动的计时器的句柄

**参数**`uint32_t Channel`

指向该定时器工作的通道

**返回值**

返回该函数执行的结果

!> 注意：当计时器在基本计时/时基(Base)模式时，没有参数`uint32_t Channel`

### HAL_TIM_XXX_Stop

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_Stop(TIM_HandleTypeDef *htim, uint32_t Channel)`

**用法**

根据具体的工作模式，停止计时器进行该方面的进程

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要停止的计时器的句柄

**参数**`uint32_t Channel`

指向该定时器工作的通道

**返回值**

返回该函数执行的结果

!> 注意：当计时器在基本计时/时基(Base)模式时，没有参数`uint32_t Channel`

### HAL_TIM_XXX_Start_IT

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_Start_IT(TIM_HandleTypeDef *htim, uint32_t Channel)`

**用法**

根据具体的工作模式，启动/使能计时器的中断

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要启动的计时器的句柄

**参数**`uint32_t Channel`

指向该定时器工作的通道

**返回值**

返回该函数执行的结果

!> 注意：当计时器在基本计时/时基(Base)模式时，没有参数`uint32_t Channel`

### HAL_TIM_XXX_Stop_IT

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_Stop_IT(TIM_HandleTypeDef *htim, uint32_t Channel)`

**用法**

根据具体的工作模式，停止计时器的中断

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要停止的计时器的句柄

**参数**`uint32_t Channel`

指向该定时器工作的通道

**返回值**

返回该函数执行的结果

!> 注意：当计时器在基本计时/时基(Base)模式时，没有参数`uint32_t Channel`

### HAL_TIM_XXX_Start_DMA

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_Start_DMA(TIM_HandleTypeDef *htim, uint32_t Channel, uint32_t *pData, uint16_t Length)`

**用法**

根据具体的工作模式，启动/使能计时器DMA传输

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要启动的计时器的句柄

**参数**`uint32_t Channel`

指向该定时器工作的通道

**参数**`uint32_t *pData`

该参数指向存放数据的缓冲区

**参数**`uint16_t Length`

该参数一般为参数`uint32_t *pData`所指向缓冲区的大小(单位：字节)

**返回值**

返回该函数执行的结果

!> 注意：当计时器在单脉冲(OnePulse)模式时，没有该函数

!> 另：在编码器(Encoder)模式下，该接口原型为`HAL_StatusTypeDef HAL_TIM_Encoder_Start_DMA(TIM_HandleTypeDef *htim, uint32_t Channel, uint32_t *pData1,uint32_t *pData2, uint16_t Length)`，参数`uint32_t *pData1`为IC1的目标缓冲区地址，参数`uint32_t *pData2`为IC2的目标缓冲区地址

### HAL_TIM_XXX_Stop_DMA

**原型：**`HAL_StatusTypeDef HAL_TIM_XXX_Stop_DMA(TIM_HandleTypeDef *htim, uint32_t Channel)`

**用法**

根据具体的工作模式，停止计时器DMA传输

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要停止的计时器的句柄

**参数**`uint32_t Channel`

指向该定时器工作的通道

**返回值**

返回该函数执行的结果

!> 注意：当计时器在单脉冲(OnePulse)模式时，没有该函数

## 其它函数

### HAL_TIM_IRQHandler

**原型：**`void HAL_TIM_IRQHandler(TIM_HandleTypeDef *htim)`

**用法**

计时器的中断处理函数，内部的定义根据该定时器的工作模式而不同

**参数**`TIM_HandleTypeDef *htim`

该参数指向触发中断的计时器的句柄

**返回值**

该函数无返回值

### HAL_TIM_OC_ConfigChannel

**原型：**`HAL_StatusTypeDef HAL_TIM_OC_ConfigChannel(TIM_HandleTypeDef *htim, TIM_OC_InitTypeDef *sConfig, uint32_t Channel)`

**用法**

用于配置指定计时器为输出比较模式，并指定输出通道，并返回该过程执行的结果

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要配置的计时器的句柄

**参数**`TIM_OC_InitTypeDef *sConfig`

该参数指向包含输出比较(OC)模式的配置信息的句柄

**参数**`uint32_t Channel`

指定输出PWM波的通道

**返回值**

返回该函数执行的结果

### HAL_TIM_PWM_ConfigChannel

**原型：**`HAL_StatusTypeDef HAL_TIM_PWM_ConfigChannel(TIM_HandleTypeDef *htim, TIM_OC_InitTypeDef *sConfig, uint32_t Channel)`

**用法**

用于配置指定计时器为PWM输出模式，并指定输出通道，并返回该过程执行的结果

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要配置的计时器的句柄

**参数**`TIM_OC_InitTypeDef *sConfig`

该参数指向包含输出比较(OC)模式的配置信息的句柄

**参数**`uint32_t Channel`

指定输出PWM波的通道

**返回值**

返回该函数执行的结果

### HAL_TIM_IC_ConfigChannel

**原型：**`HAL_StatusTypeDef HAL_TIM_IC_ConfigChannel(TIM_HandleTypeDef *htim, TIM_IC_InitTypeDef *sConfig, uint32_t Channel)`

**用法**

用于配置指定计时器为输入捕获模式，并指定输入捕获的通道，并返回该过程执行的结果

**参数**`TIM_HandleTypeDef *htim`

该参数指向需要配置的计时器的句柄

**参数**`TIM_IC_InitTypeDef *sConfig`

该参数指向包含输入捕获(IC)模式的配置信息的句柄

**参数**`uint32_t Channel`

指定捕获输入的通道

**返回值**

返回该函数执行的结果


### HAL_TIM_OnePulse_ConfigChannel

**原型：**`HAL_StatusTypeDef HAL_TIM_OnePulse_ConfigChannel(TIM_HandleTypeDef *htim, TIM_OnePulse_InitTypeDef *sConfig, uint32_t OutputChannel,  uint32_t InputChannel)`

**用法**

用于配置指定计时器为单脉冲模式，并指定输入、输出通道，并返回该过程执行的结果


**参数**`TIM_HandleTypeDef *htim`

该参数指向需要进行配置的计时器的句柄

**参数**`TIM_OnePulse_InitTypeDef *sConfig`

该参数指向包含单脉冲模式的配置信息的句柄

**参数**`uint32_t OutputChannel`

指定输入信号的通道

**参数**`uint32_t InputChannel`

指定输出PWM波的通道

**返回值**

返回该函数执行的结果


### HAL_TIM_ConfigOCrefClear

**原型：**`HAL_StatusTypeDef HAL_TIM_ConfigOCrefClear(TIM_HandleTypeDef *htim, TIM_ClearInputConfigTypeDef *sClearInputConfig, uint32_t Channel);`

**用法**

用于清除指定计时器的指定通道的配置信息

**参数**`TIM_HandleTypeDef *htim`

指定计时器的句柄

**参数**`TIM_ClearInputConfigTypeDef *sClearInputConfig`

要清除配置的结构体

**参数**`uint32_t Channel`

指定要清除的通道

**返回值**

返回该过程执行的结果

### HAL_TIM_ConfigClockSource

**原型：**`HAL_StatusTypeDef HAL_TIM_ConfigClockSource(TIM_HandleTypeDef *htim, TIM_ClockConfigTypeDef *sClockSourceConfig)`

**用法**

为指定的计时器配置时钟源

**参数**`TIM_HandleTypeDef *htim`

指定计时器的句柄

**参数**`TIM_ClockConfigTypeDef *sClockSourceConfig`

包含时钟源配置的结构体指针

**返回值**

返回该过程执行的结果


### HAL_TIM_ConfigTI1Input

**原型：**`HAL_StatusTypeDef HAL_TIM_ConfigTI1Input(TIM_HandleTypeDef *htim, uint32_t TI1_Selection)`

**用法**

为计时器输入信号TI1指定输入的通道，可以是CH1、CH2、CH3、CH4之间的XOR关系

**参数**`TIM_HandleTypeDef *htim`

指定的计时器句柄

**参数**`uint32_t TI1_Selection`

指定通道CH1、CH2、CH3、CH4

**返回值**

返回该过程执行的结果


### HAL_TIM_SlaveConfigSynchro

**原型：**`HAL_StatusTypeDef HAL_TIM_SlaveConfigSynchro(TIM_HandleTypeDef *htim, TIM_SlaveConfigTypeDef *sSlaveConfig)`

**用法**

将一个计时器配置为从计时器(从模式)

**参数**`TIM_HandleTypeDef *htim`

指定的计时器句柄

**参数**`TIM_SlaveConfigTypeDef *sSlaveConfig`

从计时器的配置项

**返回值**

返回该过程执行的结果
