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

## HAL枚举常量

对于各种外设以及HAL内核常用的枚举常量，命名形如`HAL_PPP_StructnameTypeDef`，Structname为结构体名常见的有`Status`

### HAL内核

#### HAL_StatusTypeDef

**用途**

用于表示HAL库中出现的各种状态，包括：“完成”、“错误”、“繁忙”、“超时”。

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
