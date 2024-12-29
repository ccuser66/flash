#### 基于STM32F103c8t6进行Flash读写操作 

 *  [一、题目要求][Link 1]
 *  [二、环境配置][Link 2]
 *  *  [1、RCC配置][1_RCC]
    *  [2、SYS配置][2_SYS]
    *  [3、GPIO配置][3_GPIO]
    *  [4、时钟配置][1]
    *  [5、项目管理][2]
 *  [三、代码编写][Link 3]
 *  *  [1、将flash.c和flash.h添加到工程中][1_flash.c_flash.h]
    *  [2、在main.c中添加以下代码][2_main.c]
 *  [四、仿真调试][Link 4]
 *  [五、总结][Link 5]

## 一、题目要求 

Flash地址空间的数据读取。stm32f103c8t6只有20KB 内存（RAM）供程序代码和数组变量存放，因此，针对内部Flash的总计64KB存储空间(地址从0x08000000开始），运行一次写入8KB数据，总计复位运行代码8次，将32KB数据写入Flash。并验证写入数据的正确性和读写速率。

## 二、环境配置 

使用CubeMX,Keil5

### 1、RCC配置 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ee6af225f1d6474f86cb241ac512bb9e.png)


### 2、SYS配置 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ee1c65d20b8745ca8f0d2aaff526f7f0.png)


### 3、GPIO配置 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/436000bd2220403582982cd6ad3d157b.png)


### 4、时钟配置 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b69c6f32eceb4649b835e2c26083650e.png)


### 5、项目管理 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d7e6ee544f3d4374a550e7dd02b78421.png)

（此处只列举必要操作，若需要详细步骤请参考之前的博客）

## 三、代码编写 

### 1、将flash.c和flash.h添加到工程中 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/242bdce33fa146d980a23b5801fdbf41.png)


### 2、在main.c中添加以下代码 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7ae14c6f57e04d90a47b544f60d1f4e6.png)


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/45061ccac3764a52aff5cc6c8636933c.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ca8ac2b6b6de4f4f8df97067af0c0519.png)


编译无错误

## 四、仿真调试 

连接ST-Link,烧录代码，打开魔术棒——Debug  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/03010c7819414100887017a84796a869.png)


![!\[在这里插入图片描述\](https://i-blog.csdnimg.cn/direct/37a284abd4984348b449b49b612b7e02.png](https://i-blog.csdnimg.cn/direct/c800157b6c37454ba38038ad4258532e.png)

)  
调试程序，程序运行到主函数位置

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b3f7e8e38ed64c91b79eac3a9c5653f4.png)

view——Memory Windows——Memory 1  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/36abc6bc0953453daa7013f77a6c5fe7.png)


输入0x800c00,回车

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a331332aa7b84dd4b43bc4827737ee9b.png)

View->Watch windows->Watch 1打开一个变量观察窗口，将变量FlashWBuff 和 FlashRBuff加入到 Watch 1 观察窗口：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/617eaa087c934bfaa4cb36ea4ced2f68.png)

在这里插入图片描述  
初始状态  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/229f14fdb228411797f3494bab805fdd.png)


全速运行  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/08684de4a0f84b2cb795811d6bb86e1d.png)


查看memory窗口。可以看到在FLASH地址0x0800C000区成功写入对应内容。

![](https://i-blog.csdnimg.cn/direct/977364fd084c4aedb100e274efbbbfb6.png)

断电，重新上电后再次调试，程序刚停在main入口处时还可以看到Flash对应区间的内容保持上一次写入内容值。

## 五、总结 

本次实验聚焦于 STM32F103C8T6 单片机的 Flash 读写操作。该单片机内存（RAM）仅 20KB，需合理规划数据存储，而其内部 Flash 存储空间为 64KB（起始地址 0x08000000）。  
实验中，通过 8 次复位运行，每次写入 8KB 数据来填满 64KB Flash 空间，这涉及对 Flash 编程寄存器的精确配置以确保正确写入。为验证数据，编写读取与校验程序，对比写入和读出的数据来确认准确性，同时记录读写时间以评估速率，这让我熟悉了数据校验的重要性及 Flash 读写性能的直观感受。

参考链接： [【嵌入式22】STM32F1C8T6音频数据的Flash读取与DAC播放][22_STM32F1C8T6_Flash_DAC]


[Link 1]: #_1
[Link 2]: #_3
[1_RCC]: #1RCC_5
[2_SYS]: #2SYS_7
[3_GPIO]: #3GPIO_9
[Link 3]: #_16
[1_flash.c_flash.h]: #1flashcflashh_18
[2_main.c]: #2mainc_21
[Link 4]: #_28
[Link 5]: #_53
[22_STM32F1C8T6_Flash_DAC]: https://blog.csdn.net/qq_46467126/article/details/122098829


[1]: #4_11
[2]: #5_13
