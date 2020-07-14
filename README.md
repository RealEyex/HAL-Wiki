# STM32 HAL库个人文档

**声明：**本文档将记录在STM32开发过程中所使用到的部分HAL库的内容，本文档为个人编写而非官方文档，本文档仅供参考。

## HAL库简介
HAL(Hardware Abstraction Layer)库由ST（意法半导体）公司用于帮助用户提高开发效率所开发的库。HAL库提供便于移植和面向功能的API，它对用户隐藏了MCU与外围设备的复杂性，提供ST公司提供的STM32CubeMX可以图形化的配置并初始化你的项目。

关于ST公司所提供的库：
+ 标准外设库（STD库）：是对STM32各个系列芯片的完整封装，不具有移植性，并且现在已经停止了维护。
+ LL库：相对于HAL库更加的接近底层，在寄存器级别提供了低级API，优化程度更高，但可移植性却更低。
+ HAL库：是当前ST公司主推的库，拥有良好移植性，隐藏了寄存器级别的细节，封装程度高于LL库，因此效率低于LL库

**HAL库官方文档**

+ [STM32CubeF4](http://www.st.com/en/embedded-software/stm32cubef4.html)
+ [STM32CubeF3](http://www.st.com/en/embedded-software/stm32cubef3.html)
+ [STM32CubeF2](http://www.st.com/en/embedded-software/stm32cubef2.html)
+ [STM32CubeF1](http://www.st.com/en/embedded-software/stm32cubef1.html)
+ [STM32CubeF0](http://www.st.com/en/embedded-software/stm32cubef0.html)
+ [STM32CubeL4](http://www.st.com/en/embedded-software/stm32cubel4.html)
+ [STM32CubeL1](http://www.st.com/en/embedded-software/stm32cubel1.html)
+ [STM32CubeL0](http://www.st.com/en/embedded-software/stm32cubel0.html)

## 目录链接
[数据类型](https://realeyex.github.io/HAL-Wiki/#/datatype)
[GPIO相关函数](https://realeyex.github.io/HAL-Wiki/#/gpio)
