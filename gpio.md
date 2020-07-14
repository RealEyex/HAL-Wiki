# GPIO相关函数

GPIO是通用输入/输出端口的简称，是STM32可控制的引脚。GPIO的引脚与外部硬件设备相连，可以实现与外部通讯、控制外部硬件或者采集外部硬件数据的功能。

## HAL_GPIO_Init()函数

**原型：**`void HAL_GPIO_Init(GPIO_TypeDef *GPIOx, GPIO_InitTypeDef *GPIO_Init)`

**用法：**

将参数`GPIO_Init`所指向的结构体中的值初始化`GPIOx`所指向的GPIO的配置。

**参数** `GPIO_TypeDef  *GPIOx`:

指向GPIO的GPIO_TypeDef指针

**参数**`GPIO_InitTypeDef *GPIO_Init`:

指向包含GPIO配置信息的GPIO_InitTypeDef结构体的指针


## HAL_GPIO_DeInit()函数

**原型：**`void HAL_GPIO_DeInit(GPIO_TypeDef  *GPIOx, uint32_t GPIO_Pin)`

**用法：**

将一个GPIO引脚初始化为默认值。

**参数**`GPIO_TypeDef  *GPIOx`:

需要初始化为默认值的GPIO指针

**参数**`uint32_t GPIO_Pin`:

需要初始化为默认值的引脚的引脚编号
