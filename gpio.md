# HAL库GPIO相关函数

GPIO是通用输入/输出端口的简称，是STM32可控制的引脚。GPIO的引脚与外部硬件设备相连，可以实现与外部通讯、控制外部硬件或者采集外部硬件数据的功能。

## HAL_GPIO_Init()函数

**原型：**`void HAL_GPIO_Init(GPIO_TypeDef *GPIOx, GPIO_InitTypeDef *GPIO_Init)`

**用法**

将参数`GPIO_Init`所指向的结构体中的值初始化`GPIOx`所指向的GPIO的配置。

**参数** `GPIO_TypeDef  *GPIOx`

指向GPIO的GPIO_TypeDef指针

**参数**`GPIO_InitTypeDef *GPIO_Init`

指向包含GPIO配置信息的GPIO_InitTypeDef结构体的指针

**返回值**

该函数无返回值

## HAL_GPIO_DeInit()函数

**原型：**`void HAL_GPIO_DeInit(GPIO_TypeDef  *GPIOx, uint32_t GPIO_Pin)`

**用法**

将一个GPIO引脚初始化为默认值。

**参数**`GPIO_TypeDef  *GPIOx`

需要初始化为默认值的GPIO口的指针

**参数**`uint32_t GPIO_Pin`

需要初始化为默认值的引脚的引脚编号，可以通过`|`运算符来实现对多个引脚初始化为默认值

**返回值**

该函数无返回值

## HAL_GPIO_LockPin()函数

**原型：**`HAL_StatusTypeDef HAL_GPIO_LockPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)`

**用法**

锁定一个GPIO端口的寄存器（配置），直到该GPIO端口被重置（仅对GPIOA与GPIOB有效）。

**参数**`GPIO_TypeDef* GPIOx`

需要锁定的GPIO端口，应为GPIOA或GPIOB

**参数**`uint16_t GPIO_Pin`

需要锁定的GPIO引脚编号，可以通过`|`运算符来锁定多个引脚

**返回值**

返回枚举常量`HAL_StatusTypeDef`，如果锁定成功返回`HAL_OK`否则返回`HAL_ERROR`

[枚举常量`HAL_StatusTypeDef`](../HAL-Wiki/#/datatype?id=hal_statustypedef)

## HAL_GPIO_ReadPin()函数

**原型：**`GPIO_PinState HAL_GPIO_ReadPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)`

**用法**

读取一个输入端口引脚的状态，并返回。

**参数**`GPIO_TypeDef* GPIOx`

需要读取的GPIO输入端口

**参数**`uint16_t GPIO_Pin`

需要读取的引脚所对应引脚编号

**返回值**

返回参数对应输入端口的引脚状态，返回值类型为枚举常量`GPIO_PinState`，返回值为`GPIO_PIN_RESET`或`GPIO_PIN_SET`

[枚举常量`GPIO_PinState`](../HAL-Wiki/#/datatype?id=gpio_pinstate)

## HAL_GPIO_WritePin()函数

**原型：**`GPIO_PinState HAL_GPIO_ReadPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)`

**用法**

将一个GPIO引脚置1或者置0

**参数**`GPIO_TypeDef* GPIOx`

需要设置的引脚的GPIO端口

**参数**`uint16_t GPIO_Pin`

需要设置的引脚对应的引脚编号

**返回值**

该函数无返回值

!> 此函数使用GPIOx_BSRR和GPIOx_BRR寄存器允许原子读取/修改访问。这样使得在读取和修改之间不会触发IRQ

## HAL_GPIO_TogglePin()函数

**原型：**`void HAL_GPIO_TogglePin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)`

**用法**

翻转一个GPIO引脚的值，即对该引脚的值取反

**参数**`GPIO_TypeDef* GPIO`

需要翻转的引脚的GPIO端口

**参数**`uint16_t GPIO_Pin`

需要翻转的引脚对应的引脚编号

**返回值**

该函数无返回值

## HAL_GPIO_EXTI_IRQHandler()函数

**原型：**`void HAL_GPIO_EXTI_IRQHandler(uint16_t GPIO_Pin)`

**用法**

检查指定GPIO引脚是否触发了外部中断，该函数一般由`EXTI1_IRQHandler()`函数调用

**参数**`uint16_t GPIO_Pin`

需要检查的引脚所对应的引脚编号

**返回值**

该函数无返回值

## HAL_GPIO_EXTI_Callback()回调函数

**原型：**`__weak void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)`

**用法**

该函数声明为`__weak`通常该函数应该由用户实现，该函数应该作为外部中断回调函数，处理对应引脚的外部中断

**参数**`uint16_t GPIO_Pin`

该参数表示所引发中断的引脚的引脚编号

**返回值**

该函数无需返回值
