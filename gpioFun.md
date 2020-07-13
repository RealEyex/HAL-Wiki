# GPIO相关函数

## HAL_GPIO_Init()函数

```
void HAL_GPIO_Init(GPIO_TypeDef *GPIOx, GPIO_InitTypeDef *GPIO_Init)
```
**作用**
根据GPIO_Init所指向的结构体中的值对GPIOx所指向的GPIO进行初始化。

参数 `GPIO_TypeDef  *GPIOx`:

指向GPIO的GPIO_TypeDef指针

参数`GPIO_InitTypeDef *GPIO_Init`：

指向包含GPIO配置信息的GPIO_InitTypeDef结构体的指针