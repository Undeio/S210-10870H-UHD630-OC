# S210-10870H-BCM94360Z3-OC

当前配置为 Catalina 10.15.7（19H15）的配置，要使用 BigSur，只需作些许改动即可；

OC 版本：0.76（RELEASE Versions）；

要启用跑代码模式，请执行以下操作：

1. 替换`EFI/BOOT/BOOTx64.efi`为 DEBUG 版本；
2. 替换`EFI/OC/Drivers/OpenRuntime.efi`为 DEBUG 版本；
3. 替换`EFI/OC/OpenCore.efi`为 DEBUG 版本；
4. 启用`Misc 设置`中`Debug`选项下的`AppleDebug`；
5. 请将`Misc 设置`中`Debug`选项下的`Target`设置为 67；
6. 在`NVRAM 设置`中`Add`选项下`4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14`条目的`boot-args`中添加`-v`参数；

---

## 重要提示 - 必须做的

配置文件无法开箱即用，使用前请先修改`PlatformInfo`下的机型信息：

配置文件无法开箱即用，使用前请先修改`PlatformInfo`下的机型信息：

配置文件无法开箱即用，使用前请先修改`PlatformInfo`下的机型信息：

![机型信息](https://tva1.sinaimg.cn/large/008i3skNgy1gxgn22l74xj31m70u0gsh.jpg)

三码合一，直接使用会涉嫌盗用他人信息，会封 Apple ID！！！

---

### 机器配置

|    硬件    |    型号    |      备注       |
| :--------: | :--------: | :-------------: |
|    机型    |    S210    |                 |
|    BIOS    |  AMI 5.17  |   2020/04/13    |
|   芯片组   |    未知    | 应该是 300 系列 |
|    CPU     |   10870H   |                 |
|    内存    |  16G x 2   |     雷克沙      |
|    磁盘    |     1T     |    铠侠 RD10    |
|    显卡    |  UHD 630   |                 |
|    声卡    |  ALC 235   |                 |
| 有线网卡 1 | i219v(11)  |                 |
| 有线网卡 2 |    i211    |    靠近电源     |
|  无线网卡  | BCM94360Z3 |                 |
|    蓝牙    |  同上 👆   |   走 USB 通道   |
|   显示器   |  Q2490PXQ  |       DP        |

#### 工作状态

1. BigSur 11.6（20G165）
   - 单显示输出正常（HDMI 未测试）；
   - 音频输出正常（麦克风未测试）；
   - 双有线网卡正常；
   - 无线、蓝牙正常；
   - 随航、接力、隔空投送正常；
   - USB（3.0 | 2.0 | type-c）正常；
   - 开关机、睡眠唤醒正常；
2. Catalina 10.15.7（19H15）
   - 同上

#### BIOS 设置

遵循[Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#intel-bios-settings) 的 BIOS 设置

#### 其他问题

1. 靠近`i219v(11) 网卡`(远离电源)位置的 USB3.0 不支持 USB2.0，定制两次，结果一致，至于原因，感兴趣的定制一次就知道了，通道重叠；
2. 罗技优联的键鼠会非常卡顿，与系统无关，是 2.4G 频段干扰；
   - 可以将接收器插在 USB2.0 上，其他设备插在 3.0 上，也就是说，2.0 只插优联接收器；
   - 使用 Logitech-Options 设置好键鼠后把它卸载了！！！
3. 没用过 Windows + macOS 双系统，所以产生的问题未知，也不会解决；

#### 由 Catalina 切换至 BigSur

1. Kernel 设置中的`AirPortBrcm4360_Injector.kext`：禁用或设置最大内核（已设置，无需改动）；

   ![AirPortBrcm4360_Injector.kext](https://tva1.sinaimg.cn/large/008i3skNgy1gxgniij5fvj31m70u0aj4.jpg)
   至于`BlueToolFixup.kext`和`BrcmBluetoothInjector.kext`设置的最大/小内核，适用于 Monterey，与 BigSur 无关；

2. UEFI 设置中`APFS`选项下的`MiniDate`和`MiniVersion`的值改为 -1 或 0；

   ![MiniDate 和 MiniVersion](https://tva1.sinaimg.cn/large/008i3skNgy1gxgnhfrzixj31m70u0adp.jpg)

   这里对应的应该是白苹果中的`这个安装 macOS xxx 应用程序副本已损坏，不能用来安装...`的问题，拔掉网线，在终端中设置时间应该也是可以的（未测试）；

3. Misc 设置中的`SecureBootModel`设置为`Default`或`j174`皆可；

   ![SecureBootModel](https://tva1.sinaimg.cn/large/008i3skNgy1gxgmqrumbdj31m70u0wkh.jpg)

#### 建议

0. 这只是建议，请不要较真儿；
1. 建议先在 Windows 下使用[USBToolBox](https://github.com/USBToolBox/tool)定制 USB，然后再装 macOS；
   - USBToolBox 定制分为两种情况：
     - 使用 Apple 原生类，需要根据机型定制，使用生成的 USBMap.kext 即可；
     - 使用 USBToolBox 自带的类，需要 USBToolBox.kext 和 USBMap.kext 一起使用；
   - 也可以在 macOS 下使用`USBToolBox`定制，但非常复杂，需要 USB3.0 和 USB2.0 的 U 盘，
     - 小技巧：键鼠的无线接收器一般是 2.0，使用它即可；
   - 也可以使用[Hackintool](https://github.com/headkaze/Hackintool)在 macOS 下定制，需要使用生成的 SSDT-EC-USBX.aml 和 USBPort.kext；
     - 是的，你没看错，定制后会生成专用的 SSDT-EC-USBX.aml 和 USBPort.kext；
2. 定制 USB 时，会发现使用的有 16 个端口，而 macOS 只能使用 15 个：
   - 蓝牙占用了两个，一个 3.0，一个 2.0，不敢动；
   - HDMI 旁边的 USB2.0 居然占用了一个 3.0 通道，可删除；
   - type-c 正插反插会占用两个 3.0，但只占用一个 2.0，可删除不用的 2.0；
   - 当前 USBMap.kext 删除了 USB2.0 占用的 3.0 端口；
3. 定制 USB 请注意：解除 USB 端口数量限制的`XhciPortLimit`选项在 BigSur 上无效，且勾选☑️后会导致蓝牙被禁用（有可能是个例）；

4. 想要动手尝试，定制属于自己的配置文件，请遵循[Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#intel-bios-settings)中`Intel Desktop config.plist`下`coffee-lake`的配置说明，即[Desktop Coffee Lake](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#starting-point)的详细说明；
5. 自定义内存

   作用： 关于本机中内存插槽数量错误；

   > Tips：可以先取消自定义内存选项，安装完成后，按照系统报告中自己的内存信息去填写；

   参数说明：

   ![内存信息](https://tva1.sinaimg.cn/large/008i3skNgy1gxgo3f3tclj31m70u043u.jpg)

   |      字段       |                                                说明                                                 |
   | :-------------: | :-------------------------------------------------------------------------------------------------: |
   |    DataWidth    |                                   指定内存的数据宽度，以位为单位                                    |
   | ErrorCorrection |                                指定内存支持的主要硬件纠错或检测方法                                 |
   |   FormFactor    | 指定内存的规格，在 Mac 上通常是 DIMM 或 SODIMM，台式机内存条填 9（0x09），笔记本内存条填 13（0x0D） |
   |   MaxCapacity   |                指定系统支持的最大内存量，以字节为单位（1GB = 1024 \* 1024 \* 1024）                 |
   |   TotalWidth    |   指定内存的总宽度，以位为单位，包括任何检查或纠错位；如果没有纠错位，则这个值应该等于 DataWidth    |
   |      Type       |  指定内存类型，DDR3 填 24（0x18），DDR4 填 26（0x1A），LPDDR3 填 29（0x1D），LPDDR4 填 30（0x1E）   |
   |   TypeDetail    |                               指定附加的内存类型信息，不知道就填 128                                |

   Devices 中每个字段的说明：

   |     字段      |                         说明                          |                              备注                              |
   | :-----------: | :---------------------------------------------------: | :------------------------------------------------------------: |
   |   AssetTag    |                 指定该内存的资产标签                  |                        选填，不知道填啥                        |
   |  BankLocator  |             指定内存设备所在的物理标签库              | 对应：系统报告 - 硬件 - 内存 - 内存插槽中显示的 BANK0 BANK1 等 |
   | DeviceLocator |     指定内存设备所在的物理标签插槽或主板上的位置      | 对应：系统报告 - 硬件 - 内存 - 内存插槽中显示的 DIMM0 DIMM1 等 |
   | Manufacturer  |                指定该内存设备的制造商                 |                选填，对应内存插槽详情中的制造商                |
   |  PartNumber   |                指定该内存设备的部件号                 |          选填，内存条型号，对应内存插槽详情中的部件号          |
   | SerialNumber  |                指定该内存设备的序列号                 |         选填，内存条序列号，对应内存插槽详情中的序列号         |
   |     Size      | 指定内存设备的大小，单位为 MB，0 表示该插槽未插入内存 |        把内存条的 GB 转换为 MB，1GB = 1024MB，只填数字         |
   |     Speed     |     指定设备的最大速度，单位 MHz，0 表示未知速度      |           内存条频率，2666 就填 2666，3000 就填 3000           |
