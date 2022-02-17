# [记录一下AMD平台搭建All-in-one系统-硬件选择](https://github.com/Tyrael0sun/hwblog/issues/3)

**之前有一套intel平台的all-in-one 平稳运行了OPENWRT+DSM 6.2有了2年左右。迁移到OPENWRT+QNAP也有一年。
一致遗憾之前魔改Q170时误操做将CPU的PCIE接口烧坏，无法在对其扩展其他外设（NVME或者是2.5/10G网卡）。
目前的4盘位机箱也是因为操作空间狭小，清灰和更换硬件困难，内部基热严重。这次准备一并更换掉。**

**本着花费最少升级的原则，扒拉了一下手头的硬件（R7 4750G, 32G DDR4, 512G NVME x2, AQC 108 5G网卡），
再看了看咸鱼、淘宝的硬件价格。还需要购买主板，机箱，电源，SATA扩展卡。再把A300小箱子和笔记本内存出掉回血。**
**1. 拓普龙8盘位机箱 2022升级版，不用说全网最便宜的8盘位，2022版升级了前置USB3.0 硬盘风扇改为温控，但是制作工艺没有升级，缝隙还是感人外加滴血认亲。支持1U(最长24cm安装比较困难)电源，这样就有更多的洋垃圾可以捡。**

**2. 主板选择了ASUS TUF B550M-Plus WiFi，ASUS的接口虽然不是最多的，但是所有的接口都不会有冲突。AMD平台B系列的芯片组都支持PCIE bifurcation 可以更大限度的利用PCIEX16插槽的扩展能力。但是AMD主板往QNAP里面直通硬件是个大坑。我花了3天时间只搞定了SATA接口的直通，期间在ESXI和PVE之间反复切换，一度想放弃更换为Intel平台。**

**3. 主板只有4个SATA口支持8盘位还需要SATA转接卡，综合考虑选择了M.2转5SATA的卡。我看到淘宝评论中有写到ESXI7.0中可以识别到**
<img width="529" alt="PH56" src="https://user-images.githubusercontent.com/32221824/154295088-380dbc81-ee96-443a-96dd-05fa8cbe2a84.png">

**4. 1U电源的选择比较多，最优选择是超微的PWS-441P-1H 标准1U 20cm长度方便安装，200W一下风扇不启动，80白金。可惜春节后淘宝涨价到200多。**
<img width="578" alt="PWS-441H" src="https://user-images.githubusercontent.com/32221824/154297709-d0753079-7ccf-4fda-8453-25f1c2391e31.png">
**最后只能选择了台达DPS-500YB 24cm长度 80金牌，不过56块钱的价格还能说什么呢。
以下1U电源能捡到便宜的垃圾可以考虑。都是标准1U，20cm长度，80白金**

**PWS-341P-1H 
PWS-350P-1H
PWS-441P-1H 
PWS-505P-1H 
FSP500-70UDPB
FSP500-50FSPT
FSP500-50FGGBN
FSP400-60FGGBA
FSP400-701US
FSP400-60FGGBA**

**好了需要的硬件都准备的好了，清单如下
CPU  R7 4750                                                    自备
主板  ASUS TUF B550M-PLUS WiFi              咸鱼435
内存  32G DDR4                                                 自备          
电源  DPS-500YB                                           淘宝65
网卡1 RTL8125B（主板自带）                            
网卡2 AQC108 5G （安装在PCIE 1X槽）           自备
硬盘 RC500 512G                                               自备
机箱 拓普龙8盘位                                        淘宝 510（现在有商家499）
扩展卡 M.2转SATA
接下来一篇继续写软件搭建过程和中间遇到的坑。**