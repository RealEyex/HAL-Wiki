# HAL控制函数

控制或驱动HAL内核行为的相关函数，包括`HAL_Delay()`、`HAL_IncTick()`等函数

## HAL_Init()函数

**原型：**`HAL_StatusTypeDef HAL_Init(void)`

**用法**

初始化所有的外设、flash以及Systick，该函数应该在调用其它HAL库API之前调用，一般由在mian函数中变量声明定义后马上调用。

**返回值**

返回`HAL_StatusTypeDef`结构体，用于表示初始化是否成功

[`HAL_StatusTypeDef`结构体](../HAL-Wiki/#/datatype?id=hal_statustypedef)

