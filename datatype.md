# HAL库所使用的数据类型

数据类型包括各种结构体以及枚举常量。

## 枚举常量 HAL_StatusTypeDef

**用法**

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

## 枚举常量 GPIO_PinState

**用法**

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

