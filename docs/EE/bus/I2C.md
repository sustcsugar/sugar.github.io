> 作者: sugar
> 转载请联系作者(shigang-97@qq.com ),并说明出处!

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/u010632165/article/details/109188507?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164966919916781685395687%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164966919916781685395687&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-109188507.142^v7^control,157^v4^control&utm_term=I2C&spm=1018.2226.3001.4187)

背景
--

**I²C**（**Inter-Integrated Circuit**），中文应该叫**集成电路总线**，它是一种[串行通信](https://so.csdn.net/so/search?q=%E4%B8%B2%E8%A1%8C%E9%80%9A%E4%BF%A1&spm=1001.2101.3001.7020)总线，使用多主从架构，是由飞利浦公司在 1980 年代初设计的，方便了主板、嵌入式系统或手机与周边设备组件之间的通讯。由于其简单性，它被广泛用于微控制器与传感器阵列，显示器，IoT 设备，EEPROM 等之间的通信。

**I²C** 最重要的功能包括：

*   只需要两条总线；
*   所有组件之间都存在简单的主 / 从关系，连接到总线的每个设备均可通过唯一地址进行软件寻址；
*   I²C 是真正的多主设备总线，可提供仲裁和冲突检测；
*   传输速度；
    *   标准模式：Standard Mode = 100 `Kbps`
    *   快速模式：Fast Mode = 400 `Kbps`
    *   高速模式： High speed mode = 3.4 `Mbps`
    *   超快速模式： Ultra fast mode = 5 `Mbps`
*   最大主设备数：无限制；
*   最大从机数：理论上是 127；

以上是 I²C 的一些重要特点，下面会进一步对 I²C 进行介绍。

硬件层
---

**I²C** 协议仅需要一个 **SDA 和 SCL** 引脚。SDA 是**串行数据线的**缩写，而 SCL 是**串行时钟线的**缩写。这两条数据线需要接上拉电阻。

设备间的连接如下所示：

![](https://img-blog.csdnimg.cn/20201020201901557.png#pic_center)

使用 I²C，可以将多个从机（`Slave`）连接到单个主设备（`Master`），并且还可以有多个主设备（`Master`）控制一个或多个从机（`Slave`）。

> 假如希望有多个微控制器（`MCU`）将数据记录到单个存储卡或将文本显示到单个 LCD 时，这个功能就非常有用。

I²C 总线（`SDA`，`SCL`）内部都使用漏极开路驱动器（开漏驱动），因此`SDA`和`SCL` **可以被拉低为低电平，但是不能被驱动为高电平**，所以每条线上都要使用一个上拉电阻，默认情况下将其保持在高电平；

![](https://img-blog.csdnimg.cn/2020102020201146.png#pic_center)

上**拉电阻的**值取决于许多因素。[德州仪器 TI 建议](https://www.ti.com/lit/an/slva689/slva689.pdf) 使用以下公式来计算正确的上拉电阻值：

$$
R_p(min)=\cfrac{V_{DD}-V_{OL}(max)}{I_{OL}} 
$$
$$
R p ( m i n ) =\cfrac{t_r}{0.8473xC_b} 
$$

其中 $V_{OL}$是逻辑低电压；

$I_{OL}$是逻辑低电流；

$t_r$是信号的最大上升时间；

$C_b$是总线（电线）电容；

具体如下所示：

![](https://img-blog.csdnimg.cn/20201020202043786.png#pic_center)

从上表可知，使用 I2C 设备必须在 3 mA的灌电流下工作，

这里不难发现需要在做选型需要满足几个条件；

*   灌电流 最大值为 3 mA；
*   另外 I2C 总线规范和用户手册还为低电平输出电压 $V_{OL}$ 设置了最大值为 0.4V

$$R_p(min) = \cfrac{VCC - 0.4V}{3mA}$$

所以根据上述公式可以计算，对于 5V 的电源，每个上拉电阻必须至少具有 1.53kΩ，而对于 3.3V 的电源，每个电阻必须至少具有 967Ω。

如果觉得计算电阻值比较麻烦，也可以使用**典型值 4.7kΩ**。

> 上述推导过程可以参考 TI 的文档《I2C Bus Pullup Resistor Calculation》 https://www.ti.com/lit/an/slva689/slva689.pdf

最终在调试的时候，当我们测量 SDA 或 SCL 信号并且逻辑 LOW 上的电压高于 0.4V 时，我们就知道可以知道灌电流太高了；

![](https://img-blog.csdnimg.cn/20201020202110799.png#pic_center)

当然，这并不意味着每当灌电流超过 3mA 时，设备就会立即停止工作。但是，在操作超出其规格的设备时，应始终小心，因为它可能导致通信故障，缩短其使用寿命甚至甚至永久损坏设备。

数据传输协议
------

主设备和从设备进行数据传输时遵循以下协议格式。数据通过一条 SDA 数据线在主设备和从设备之间传输`0`和`1`的串行数据。串行数据序列的结构可以分为，开始条件，地址位，读写位，应答位，数据位，停止条件，具体如下所示；  
![](https://img-blog.csdnimg.cn/20201020202130485.jpg#pic_center)

**开始条件**

当主设备决定开始通讯时，需要发送开始信号，需要执行以下动作；

*   先将 `SDA` 线从高压电平切换到低压电平；
*   然后将`SCL`从高电平切换到低电平；

在主设备发送`开始条件`信号之后，所有从机即使处于睡眠模式也将**变为活动状态**，并**等待接收地址位**。

具体如下图所示；

![](https://img-blog.csdnimg.cn/20201020202551889.jpg#pic_center)

**地址位**

通常地址位占 7 位数据，主设备如果需要向从机发送 / 接收数据，首先要发送对应从机的地址，然后会匹配总线上挂载的从机的地址；

> I2C 还支持 10 位寻址；

**读写位**

该位指定数据传输的方向；

*   如果主设备需要将数据发送到从设备，则该位设置为 `0`；
*   如果主设备需要往从设备接收数据，则将其设置为 `1` 。

**ACK / NACK**

主机每次发送完数据之后会等待从设备的应答信号`ACK`；

*   在第 9 个时钟信号，如果从设备发送应答信号`ACK`，则`SDA`会被拉低；
*   若没有应答信号`NACK`，则`SDA`会输出为高电平，这过程会引起主设备发生重启或者停止；

![](https://img-blog.csdnimg.cn/20201020202650973.jpg#pic_center)

**数据块**

传输的数据总共有 8 位，由发送方设置，它需要将数据位传输到接收方。

发送之后会紧跟一个`ACK` / `NACK`位，如果接收器成功接收到数据，则设置为 0。否则，它保持逻辑 “1”。

重复发送，直到数据完全传输为止。

**停止条件**

当主设备决定结束通讯时，需要发送开始信号，需要执行以下动作；

*   先将 SDA 线从低电压电平切换到高电压电平；
*   再将 SCL 线从高电平拉到低电平；

具体如下图所示；

![](https://img-blog.csdnimg.cn/20201020202710717.jpg#pic_center)

实际上如何工作？
--------

**第一步：起始条件**

主设备通过将 SDA 线从高电平切换到低电平，再将 SCL 线从高电平切换到低电平，来向每个连接的从机发送启动条件 ：

![](https://img-blog.csdnimg.cn/2020102020274841.png#pic_center)

**第二步：发送从设备地址**

主设备向每个从机发送要与之通信的从机的 7 位或 10 位地址，以及相应的**读 / 写位**；

![](https://img-blog.csdnimg.cn/20201020202848865.png#pic_center)

**第三步：接收应答**

每个从设备将主设备发送的地址与其自己的地址进行比较。如果地址匹配，则从设备通过**将 SDA 线拉低一位以表示返回一个 ACK 位**；

如果来自主设备的地址与从机自身的地址不匹配，则**从设备将 SDA 线拉高，表示返回一个 NACK 位**；

![](https://img-blog.csdnimg.cn/20201020202902121.png#pic_center)

**第四步：收发数据**

主设备发送或接收数据到从设备；

![](https://img-blog.csdnimg.cn/20201020203115562.png#pic_center)

**第五步：接收应答**

在传输完每个数据帧后，接收设备将另一个 ACK 位返回给发送方，以确认已成功接收到该帧：

![](https://img-blog.csdnimg.cn/20201020203045290.png#pic_center)

**第六步：停止通信**

为了停止数据传输，主设备将 SCL 切换为高电平，然后再将 SDA 切换为高电平，从而向从机发送停止条件；

![](https://img-blog.csdnimg.cn/20201020202931538.png#pic_center)

单个主设备连接多个从机
-----------

I2C 总线上的主设备使用 7 位地址对从设备进行寻址，可以使用 128（ 2 7 2^7 27）个从机地址。

> 请使用 4.7K 上拉电阻将 SDA 和 SCL 线连接到 Vcc；

![](https://img-blog.csdnimg.cn/20201020203134262.png#pic_center)

多个主设备连接多个从机
-----------

多个主设备可以连接到一个或多个从机；

当两个主设备试图通过 SDA 线路同时发送或接收数据时，同一系统中的多个主设备就会出现问题。

为了解决这个问题，**每个主设备都需要在发送消息之前检测 SDA 线是低电平还是高电平**；

*   如果 SDA 线为低电平，则意味着另一个主设备可以控制总线，并且主设备应等待发送消息。
    
*   如果 SDA 线为高电平，则可以安全地发送消息。
    
    > 要将多个主设备连接到多个从机，请使用下图，其中 4.7K 上拉电阻将 SDA 和 SCL 线连接到 Vcc：
    

![](https://img-blog.csdnimg.cn/20201020203152577.png#pic_center)

如何编程？
-----

Talk is cheap. Show me the code.

参考了 STM32 的 HAL 库中 I2C 驱动，主设备发送函数`HAL_I2C_Master_Transmit()`具体如下：

```
/**
  * @brief  Transmits in master mode an amount of data in blocking mode.
  * @param  hi2c Pointer to a I2C_HandleTypeDef structure that contains
  *                the configuration information for the specified I2C.
  * @param  DevAddress Target device address: The device 7 bits address value
  *         in datasheet must be shifted to the left before calling the interface
  * @param  pData Pointer to data buffer
  * @param  Size Amount of data to be sent
  * @param  Timeout Timeout duration
  * @retval HAL status
  */
HAL_StatusTypeDef HAL_I2C_Master_Transmit(I2C_HandleTypeDef *hi2c, 
                                          uint16_t DevAddress, 
                                          uint8_t *pData, 
                                          uint16_t Size, 
                                          uint32_t Timeout){
  uint32_t tickstart = 0x00U;

  /* Init tickstart for timeout management*/
  tickstart = HAL_GetTick();

  if(hi2c->State == HAL_I2C_STATE_READY){
    /* Wait until BUSY flag is reset */
    if(I2C_WaitOnFlagUntilTimeout(hi2c, I2C_FLAG_BUSY, SET, I2C_TIMEOUT_BUSY_FLAG, tickstart) != HAL_OK){
      return HAL_BUSY;
    }

    /* Process Locked */
    __HAL_LOCK(hi2c);

    /* Check if the I2C is already enabled */
    if((hi2c->Instance->CR1 & I2C_CR1_PE) != I2C_CR1_PE){
      /* Enable I2C peripheral */
      __HAL_I2C_ENABLE(hi2c);
    }

    /* Disable Pos */
    hi2c->Instance->CR1 &= ~I2C_CR1_POS;

    hi2c->State     = HAL_I2C_STATE_BUSY_TX;
    hi2c->Mode      = HAL_I2C_MODE_MASTER;
    hi2c->ErrorCode = HAL_I2C_ERROR_NONE;

    /* Prepare transfer parameters */
    hi2c->pBuffPtr    = pData;
    hi2c->XferCount   = Size;
    hi2c->XferOptions = I2C_NO_OPTION_FRAME;
    hi2c->XferSize    = hi2c->XferCount;

    /* Send Slave Address */
    if(I2C_MasterRequestWrite(hi2c, DevAddress, Timeout, tickstart) != HAL_OK){
      if(hi2c->ErrorCode == HAL_I2C_ERROR_AF){
        /* Process Unlocked */
        __HAL_UNLOCK(hi2c);
        return HAL_ERROR;
      }else{
        /* Process Unlocked */
        __HAL_UNLOCK(hi2c);
        return HAL_TIMEOUT;
      }
    }

    /* Clear ADDR flag */
    __HAL_I2C_CLEAR_ADDRFLAG(hi2c);

    while(hi2c->XferSize > 0U){
      /* Wait until TXE flag is set */
      if(I2C_WaitOnTXEFlagUntilTimeout(hi2c, Timeout, tickstart) != HAL_OK){
        if(hi2c->ErrorCode == HAL_I2C_ERROR_AF){
          /* Generate Stop */
          hi2c->Instance->CR1 |= I2C_CR1_STOP;
          return HAL_ERROR;
        }else{
          return HAL_TIMEOUT;
        }
      }
      /* Write data to DR */
      hi2c->Instance->DR = (*hi2c->pBuffPtr++);
      hi2c->XferCount--;
      hi2c->XferSize--;

      if((__HAL_I2C_GET_FLAG(hi2c, I2C_FLAG_BTF) == SET) 
         && (hi2c->XferSize != 0U)){
        /* Write data to DR */
        hi2c->Instance->DR = (*hi2c->pBuffPtr++);
        hi2c->XferCount--;
        hi2c->XferSize--;
      }
      /* Wait until BTF flag is set */
      if(I2C_WaitOnBTFFlagUntilTimeout(hi2c, Timeout, tickstart) != HAL_OK){
          
        if(hi2c->ErrorCode == HAL_I2C_ERROR_AF){
          /* Generate Stop */
          hi2c->Instance->CR1 |= I2C_CR1_STOP;
          return HAL_ERROR;
        }else{
          return HAL_TIMEOUT;
        }
      }
    }

    /* Generate Stop */
    hi2c->Instance->CR1 |= I2C_CR1_STOP;

    hi2c->State = HAL_I2C_STATE_READY;
    hi2c->Mode = HAL_I2C_MODE_NONE;
    
    /* Process Unlocked */
    __HAL_UNLOCK(hi2c);

    return HAL_OK;
  }else{
    return HAL_BUSY;
  }
}
```

总结
--

本文主要介绍 I2C 的入门基础知识，从 I2C 协议的硬件层，协议层进行了简单介绍；作者能力有限，难免存在错误和纰漏，请大佬不吝赐教。