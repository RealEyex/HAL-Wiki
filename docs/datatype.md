# HAL库数据结构与宏

在HAL库中针对各种各样的外设，封装了大量的数据结构，其中包括各种结构体以及枚举常量。

!> 在本文中的结构体名中出现的**PPP**表示外设的名称，比如GPIO、USART、ADC等

## HAL库数据结构规范
HAL驱动程序通常包含以下数据结构：
+ 指向外设的句柄结构
+ 用于初始化/配置外设的结构体
+ 用于特殊过程的结构体

## HAL句柄

形如`PPP_HandleTypeDef *`的结构体指针称之为句柄，句柄是在HAL驱动程序中实现的主要结构，句柄保存外设所需要的所有的配置信息，可以用同一个句柄去初始化多个同类型的外设，命名形如`PPP_HandleTypedef`

!> 对于一部分特殊的外设，HAL库并未使用句柄(GPIO、SYSTICK、NVIC、PWR、RCC、FLASH)

## HAL结构体

HAL结构体包括初始化结构体与配置结构体，用于保存某种外设的各种参数，常用于初始化某个外设，命名形如`PPP_InitTypeDef`

### GPIO相关

#### GPIO_InitTypeDef

**用途**

用于配置GPIO引脚

**定义**
```c
typedef struct
{
  uint32_t Pin;         // 指定要配置的GPIO引脚编号
  uint32_t Mode;        // 指定所选引脚的工作模式
  uint32_t Pull;        // 指定所选引脚的输入状态(上拉、下拉或浮空)
  uint32_t Speed;       // 指定所选引脚的输出速度
  uint32_t Alternate;   // 指定所选引脚的复用功能
}GPIO_InitTypeDef;
```

### TIM相关

#### TIM_HandleTypeDef

**用途**

用于初始化一个计时器

**定义**
```c
typedef struct
{
  TIM_TypeDef                 *Instance;      // TIM的寄存器地址
  TIM_Base_InitTypeDef        Init;           // 配置计时器为时基的必要结构体
  HAL_TIM_ActiveChannel       Channel;        // 计时器的输出通道
  DMA_HandleTypeDef           *hdma[7];       // DMA句柄
  HAL_LockTypeDef             Lock;           // 是否上锁
  __IO HAL_TIM_StateTypeDef   State;          // 计时器的状态

} TIM_HandleTypeDef;
```

!> 该结构体还有第7个成员，当用户需要自定义回调函数时，添加宏定义`#define USE_HAL_TIM_REGISTER_CALLBACKS 1`，编译器通过条件编译将该成员加入该结构体


#### TIM_Base_InitTypeDef

**用途**

用于配置计时器用于时基

**定义**
```c
typedef struct
{
  uint32_t Prescaler;                 // 预分频器的值，即预分频系数
  uint32_t CounterMode;               // 计时器计时模式(向上、向下、向中对齐)
  uint32_t Period;                    // 计时器自动重装载值，即一个计时周期的值
  uint32_t ClockDivision;             // 时钟分频因子，一般用于输入捕获所使用的采样频率
  uint32_t RepetitionCounter;         // 重复次数
  uint32_t AutoReloadPreload;         // 是否使能自动重载预加载
} TIM_Base_InitTypeDef;
```

!> 对于该结构体的`uint32_t RepetitionCounter;`成员，该成员仅在初始化高级计时器时可用，使用通用计时器时该成员无效

#### TIM_OC_InitTypeDef

**用途**

配置计时器用于OC(输出比较)生成PWM波

**定义**

```c
typedef struct
{
  uint32_t OCMode;              // 设置定时器输出的模式
  uint32_t Pulse;               // 设置输出比较值
  uint32_t OCPolarity;          // 指定有效输出的电平(高/低)
  uint32_t OCNPolarity;         // 指定互补输出时有效输出的电平(高/低)
  uint32_t OCFastMode;          // 是否启用快速模式，用于加快OC对触发输入事件的响应
  uint32_t OCIdleState;         // 指定空闲状态下TIM的输出比较引脚的状态
  uint32_t OCNIdleState;        // 指定空闲状态下TIM的-互补-输出比较引脚的状态
} TIM_OC_InitTypeDef;
```

!> 对于互补输出相关配置，只对高级计时器有效

#### TIM_OnePulse_InitTypeDef

**用途**

配置计时器用于单脉冲输出(OPM)

**定义**

```c
typedef struct
{
  uint32_t OCMode;              // 设置定时器输出的模式
  uint32_t Pulse;               // 设置输出比较值
  uint32_t OCPolarity;          // 指定有效输出的电平(高/低)
  uint32_t OCNPolarity;         // 指定互补输出时有效输出的电平(高/低)
  uint32_t OCIdleState;         // 指定空闲状态下TIM的输出比较引脚的状态
  uint32_t OCNIdleState;        // 指定空闲状态下TIM的-互补-输出比较引脚的状态
  uint32_t ICPolarity;          // 指定输入信号的有效沿(上升沿/下降沿/上升沿和下降沿)
  uint32_t ICSelection;         // 设置IC与TI之间的映射关系
  uint32_t ICFilter;            // 输入捕获过滤值
} TIM_OnePulse_InitTypeDef;
```

#### TIM_IC_InitTypeDef

**用途**

配置计时器用于输入捕获(IC)

**定义**
```c
typedef struct
{
  uint32_t  ICPolarity;         // 指定输入信号的有效沿(上升沿/下降沿/上升沿和下降沿)
  uint32_t ICSelection;         // 设置IC与TI之间的映射关系
  uint32_t ICPrescaler;         // 设置输入捕获预分频系数
  uint32_t ICFilter;            // 设置输入捕获过滤器(输入捕获滤波)
} TIM_IC_InitTypeDef;
```

#### TIM_Encoder_InitTypeDef

**用途**

配置计时器用作编码器

**定义**

```c
typedef struct
{
  uint32_t EncoderMode;           // 指定编码器工作模式 
  uint32_t IC1Polarity;           // 指定IC1输入信号的有效沿(上升沿/下降沿/上升沿和下降沿)
  uint32_t IC1Selection;          // 设置IC1与TI1之间的映射关系
  uint32_t IC1Prescaler;          // 设置IC1输入捕获预分频系数
  uint32_t IC1Filter;             // 设置IC1输入捕获过滤器(输入捕获滤波)
  uint32_t IC2Polarity;           // 指定IC2输入信号的有效沿(上升沿/下降沿/上升沿和下降沿)
  uint32_t IC2Selection;          // 设置IC2与TI2之间的映射关系
  uint32_t IC2Prescaler;          // 设置IC2输入捕获预分频系数
  uint32_t IC2Filter;             // 设置IC2输入捕获过滤器(输入捕获滤波)
} TIM_Encoder_InitTypeDef;
```

#### TIM_ClockConfigTypeDef

**用途**

用于配置计时器的时钟源

**定义**

```c
typedef struct
{
  uint32_t ClockSource;         // 指定时钟源
  uint32_t ClockPolarity;       // 指定时钟源极性
  uint32_t ClockPrescaler;      // 设置时钟源的预分频系数
  uint32_t ClockFilter;         // 设置时钟源的过滤器
} TIM_ClockConfigTypeDef;
```

#### TIM_ClearInputConfigTypeDef

**用途**

用于清除计时器的输入模式配置

**定义**

```c
typedef struct
{
  uint32_t ClearInputState;         // 清除指定的输入状态
  uint32_t ClearInputSource;        // 清除指定的输入时钟源
  uint32_t ClearInputPolarity;      // 清除指定的输入极性
  uint32_t ClearInputPrescaler;     // 清除设置的输入预分频器的值
  uint32_t ClearInputFilter;        // 清除设置的输入过滤器的值
} TIM_ClearInputConfigTypeDef;
```

#### TIM_MasterConfigTypeDef

**用途**

用于计时器的主从模式下，主计时器的配置

**定义**

```c
typedef struct
{
  uint32_t  MasterOutputTrigger;              // 指定主计时器的TRGO(触发输出)
#if defined(TIM_CR2_MMS2)
  uint32_t  MasterOutputTrigger2;             // 该参数在高级定时器上可用，指定主计时器的TRGO2(触发输出)
#endif
  uint32_t  MasterSlaveMode;                  // 是否启用(使能)主从模式
} TIM_MasterConfigTypeDef;
```

#### TIM_SlaveConfigTypeDef

**用途**

用于计时器的主从模式下，从计时器的配置

**定义**

```c
typedef struct
{
  uint32_t  SlaveMode;                    // 指定计时器的从模式
  uint32_t  InputTrigger;                 // 指定输入触发源
  uint32_t  TriggerPolarity;              // 指定触发信号的有效沿(上升沿/下降沿/上升沿和下降沿)
  uint32_t  TriggerPrescaler;             // 设置触发信号预分频系数
  uint32_t  TriggerFilter;                // 设置触发信号的过滤器
} TIM_SlaveConfigTypeDef;
```

#### TIM_SlaveConfigTypeDef

**用途**

用于计时器的主从模式下，从计时器的配置

**定义**

```c
typedef struct
{
  uint32_t OffStateRunMode;                 // 运行模式下OC/OCN输出控制
  uint32_t OffStateIDLEMode;                // 空闲模式下OC/OCN输出控制
  uint32_t LockLevel;                       // 设置锁定
  uint32_t DeadTime;                        // 设置死区时间
  uint32_t BreakState;                      // 设置是否使能刹车
  uint32_t BreakPolarity;                   // 刹车输入极性(高低电平)
  uint32_t BreakFilter;                     // 刹车输入过滤器
#if defined(TIM_BDTR_BK2E)          // 刹车输入信号2配置
  uint32_t Break2State;                 
  uint32_t Break2Polarity;              
  uint32_t Break2Filter;                
#endif /*TIM_BDTR_BK2E */
  uint32_t AutomaticOutput;                 // 是否启用(使能)自动输出
} TIM_BreakDeadTimeConfigTypeDef;
```


## HAL枚举常量

对于各种外设以及HAL内核常用的枚举常量，命名形如`HAL_PPP_StructnameTypeDef`，Structname为结构体名常见的有`Status`

### HAL内核

#### HAL_StatusTypeDef

**用途**

用于表示HAL库执行某个过程中可能出现的各种状态，包括：“完成”、“错误”、“繁忙”、“超时”。

**定义**
```c
typedef enum 
{
  HAL_OK       = 0x00U,
  HAL_ERROR    = 0x01U,
  HAL_BUSY     = 0x02U,
  HAL_TIMEOUT  = 0x03
} HAL_StatusTypeDef;
```

#### HAL_LockTypeDef

**用途**

防止出现资源抢占，比如：当调用`HAL_UART_Transmit()`函数发送数据时会通过`__HAL_LOCK()`来将UART实例的句柄设置为`HAL_LOCKED`状态，当数据未发送完成时再次调用该函数会返回`HAL_BUSY`，从而避免出现未知的错误

**定义**
```c
typedef enum 
{
  HAL_UNLOCKED = 0x00U,
  HAL_LOCKED   = 0x01  
} HAL_LockTypeDef;
```

#### HAL_TickFreqTypeDef

**用途**

仅用于表示Systick的频率

**定义**
```c
typedef enum
{
  HAL_TICK_FREQ_10HZ         = 100U,
  HAL_TICK_FREQ_100HZ        = 10U,
  HAL_TICK_FREQ_1KHZ         = 1U,
  HAL_TICK_FREQ_DEFAULT      = HAL_TICK_FREQ_1KHZ
} HAL_TickFreqTypeDef;
```

### GPIO相关

#### GPIO_PinState

**用途**

用来表示某个引脚的状态（0或1）

**定义**
```c
typedef enum
{
  GPIO_PIN_RESET = 0U,
  GPIO_PIN_SET
}GPIO_PinState;
```

> 从定义中我们可以看到HAL库的开发者是用SET表示1、RESET表示0，即“设置引脚”表示将该引脚置1，“复位引脚”表示将该引脚置0，在HAL库的文档中时常这样描述对引脚的置1或置0，而在本文档中使用置1或置0来描述。


### TIM相关

#### HAL_TIM_StateTypeDef

**用途**

用来表示计时器当前的状态

**定义**
```c
typedef enum
{
  HAL_TIM_STATE_RESET             = 0x00U,        // 未进行初始化或被禁用
  HAL_TIM_STATE_READY             = 0x01U,        // 就绪
  HAL_TIM_STATE_BUSY              = 0x02U,        // 忙碌
  HAL_TIM_STATE_TIMEOUT           = 0x03U,        // 超时
  HAL_TIM_STATE_ERROR             = 0x04U         // 错误
} HAL_TIM_StateTypeDef;
```

#### HAL_TIM_ActiveChannel

**用途**

用来设置计时器通道的开启与关闭

**定义**
```c
typedef enum
{
  HAL_TIM_ACTIVE_CHANNEL_1        = 0x01U,            // 通道1
  HAL_TIM_ACTIVE_CHANNEL_2        = 0x02U,            // 通道2
  HAL_TIM_ACTIVE_CHANNEL_3        = 0x04U,            // 通道3
  HAL_TIM_ACTIVE_CHANNEL_4        = 0x08U,            // 通道4
#if defined(TIM_CCER_CC5E)
  HAL_TIM_ACTIVE_CHANNEL_5        = 0x10U,            // 通道5
#endif
#if defined(TIM_CCER_CC6E)
  HAL_TIM_ACTIVE_CHANNEL_6        = 0x20U,            // 通道6
#endif
  HAL_TIM_ACTIVE_CHANNEL_CLEARED  = 0x00U             // 不选择任何通道
} HAL_TIM_ActiveChannel;
```


## 宏定义

HAL库所提供的与外设功能相关的宏，这些宏定义在对应外设的头文件中

```c
__HAL_PPP_ENABLE_IT(__HANDLE__, __INTERRUPT__)      // 启用某个外设的中断
__HAL_PPP_DISABLE_IT(__HANDLE__,__INTERRUPT__)      // 禁用某个外设的中断
__HAL_PPP_GET_IT(__HANDLE__, __INTERRUPT__)         // 获取某个外设的中断状态
__HAL_PPP_CLEAR_IT(__HANDLE__, __INTERRUPT__)       // 清除某个外设的中断状态
__HAL_PPP_GET_FLAG(__HANDLE__, __FLAG__)            // 获取某个外设的标志位状态
__HAL_PPP_CLEAR_FLAG (__HANDLE__, __FLAG__)         // 清除某个外设的标志位状态
__HAL_PPP_ENABLE(__HANDLE__)                        // 启用某个外设
__HAL_PPP_DISABLE(__HANDLE__)                       // 禁用某个外设
__HAL_PPP_GET_IT_SOURCE(__HANDLE__, __INTERRUPT__)  // 检查指定的中断源
```
