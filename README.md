# smartCard

### 简介：使用nRF52840做主控，集成蓝牙BLE，NFC，TF卡，LED，BUTTON，e-paper，SWD下载，usbmicro充电，DFU等功能的多功能一体化卡片，卡片大小仅34 * 51 mm

### 功能：nfc模拟mifare classic 1K卡片的功能，可以实现软件模拟多卡的功能并通过按钮切换。蓝牙可以向卡片传输图片并保存在TF卡中，nrf52840取出并显示在e-paper上，可以通过DFU更新固件。
https://hackaday.io/project/169967-nrf52-smartwatch

nrf52840 + 电子墨水屏

部件：


9-28：下载AD，学会认证免费使用半年，找到MDBT50Q-1MV2的原理图和封装，找到M41T62Q6F的原理图和封装。
		阅读nRF52940技术手册。

E104-BT5032A

搞懂openpose的输出
Nsight-sys性能分析测试
生成数据集

微雪1.54inch墨水屏采用spi进行通信：
	其中，CS作为从机片选段，需要一直低电平（使能），墨水屏才能工作。
	DC：数据/命令控制引脚，当DC=0时写入命令；DC=1时写入数据。
	时序：CPHL=0， CPOL=0 （SPI0模式）
	时钟空闲时为低电平，且在串行同步时钟的第一个跳变沿(上升或下降)数据被采样。

Smart Card
 nRF52840 MAC地址：C7：77：97：7D：65：60
开发板coreMAC地址：D0：4A：81：A5：A5：D3

第二版PCB有问题的点：
	micro口下载
	显示电路
	NFC
解决办法：重新焊接一个新板子，单元测试，先焊接好供电，然后先测试下载，再测试NFC，最后测试显示电路。

生日礼物：
	① 重制一版PCB，墨水屏驱动电路采用驱动板上的电路，布局布线排开，SWD下载线引出，采用1.54inch墨水屏，micro口充电，蓝牙不覆铜，NFC电路照常，按键和LED（输入输出人机交互）电路照常，供电电路照常，充电电路照常。电阻电容采用0603封装，换一个可以保持两种状态的开关。去掉电阻电容的丝印，保留插画（可以对着电路图焊接）。由于NRF52840的板载内存是256KB，flash是1MB，对于存储图片着实有些着急，所以焊接一个板载内存模块，外置flash。在反面焊接一个SD卡槽（具体可以参考为微雪开发板的SD电路（使用SPI进行通信））。
	② 购买电烙铁（送锡丝和助焊剂），jlink，电路用到的元件分别买，购买万用表。
	③ 购买的元件包括微雪的nRF52840模块，下载器，1.54inch墨水屏，micro口，电容电阻电感磁珠二极管三极管，按键开关，电池。四个led和三个按键。
	
	1. 硬件设计：
	一、供电模块（带充电）：
		MCP73831T-4ADI/OT
		
		
		
	二、降压模块（5V转3.3V）：
		TPS61221DCKT
		
		
		
		因为降压电路只负责简单的锂电池直流降压工作，所以去除了两个10uF的旁路电容。
	三、核心控制电路：
		
	四、按键部分电路（输入）：
		
	五、LED部分电路（输出）：
		
	六、墨水屏驱动电路
		
		
	七、NFC电路
		
	八、SD卡槽电路
		
	九、下载与调试电路
		
	原理图完成：
	
	PCB初版绘制完成：
	
	
	排版后：
	
	
	排线后：
	
	3D:
	
	
	
	发到PCB工厂出品实物：
	
	
	
	发现降压电路有缺陷，重新设计了一款pcb并出板。
	降压电路改为RT9193，micro口改为带加固的封装。
	
	
	
	2. 采购元件
	关于micro座子，充电模块和3.3v稳压模块已经焊完，led部分和核心板都已经焊上去，NFC部位的两个电容也已经焊上去。按钮最后焊，但是肯定没问题，24pin的座子焊的时候花点功夫，不过也肯定没问题。
	回去先把0.1uF的电容和6个10K的电阻焊上去，最后再使用万用表验证一遍焊接的稳定性。
	剩下需要购买的有：
		a. TF卡槽
		b. 显示模块中的
			i. 三个二极管 MBR0530
			ii. Si1308EDL
			iii. 68uH的电感，贴片4mm*4mm
			iv. 2个4.7uF 50V的电容
		c. 公对公排针
	已买
	终于找到了TF卡的封装原形，molex的49225-0821
	
			
			
	3. 焊接
	焊接和采购原件已同时进行。
	4. 硬件调试
	硬件的初步调试工作在焊接同时已用万用表测试。
	nRF52840控制和下载没问题
	led测试没问题
	NFC测试正常
	按钮设计错误
	蓝牙测试正常：MAC地址D0:4A:81:A5:A5:D3
	显示测试：显示测试没问题
	TF卡测试
	这一版按钮连接线出现了问题，显示接口反了，需要买个上下都有的24pin排针，主要用下面，否则要重新设计PCB，将按钮换成自锁按钮。其他地方都没问题，焊接后图片如下：
	5. 
	
	
	6. 软件设计
	需等硬件调试完毕后再进行软件测试。
	重写成gcc版本。
	蓝牙 nfc 显示成功集成在了一个程序中，下一步写显示接口，供调试使用。
	显示接口加入成功，可以显示中英文字符等调试信息。
	下载android studio，编写app，传递二值化后图片信息并刷新显示。
	
	当nrf_sdh_ble_enable出现err_code为4的错误时，打印返回地址值，重新设置与打印值相同的ld文件起始值就可以使代码运行正常。
	
	使用wireshark进行蓝牙抓包和分析。 536882160 -> 20002bf0 3d410

	现在已经可以做到手机app二值化选择的图片并且传输给smart card显示。
	
	4.7UF电容虚焊，把nRF_Log调出来再做别的。
	
	功能简化
	显示屏 + nfc
	一个reset按钮
	
改变：
	使用中景园1.54inch LCD显示屏
	NFC变成外接天线
	电池变成纽扣电池
尺寸进一步变小![image](https://user-images.githubusercontent.com/24819521/144950962-bb27b810-61f2-4dec-b710-68e5b3885197.png)
