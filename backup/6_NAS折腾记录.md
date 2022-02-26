# [NAS折腾记录](https://github.com/Tyrael0sun/hwblog/issues/6)

本来是要写AMD的平台如何直通SATA，RTL8125B在ESXI7.0的使用经验，QNAP如何编写配置文件。无奈体验了一下DSM7.0.1简直太流畅了，相比去QNAP任何一项操作都在不停的转圈圈。这个感受让我立马放弃了继续折腾黑威，以上的这些折腾都是白费了。但想想既然折腾过就把过程写出来，以避免后人继续掉坑。

### 首先是AMD平台直通

**建议以后玩ESXI虚拟化最好直接上intel平台，不要在AMD平台浪费时间。**
PVE上AMD的直通主要是增加IOMMU参数，使用ACS功能让IOMMU重新分组以下是修改方法：
nano /etc/default/grub
将GRUB_CMDLINE_LINUX_DEFAULT="quiet"修改为 GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on pcie_acs_override=downstream,multifunction video=vesafb:off video=efifb:off"
update-grub
nano etc/modules

增加如下参数
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd

update-initramfs -u -k all
使用下面的命令查询你需要直通的设配在那个IOMMU的group
!/bin/bash
shopt -s nullglob
for g in `find /sys/kernel/iommu_groups/* -maxdepth 0 -type d | sort -V`; do
    echo "IOMMU Group ${g##*/}:"
    for d in $g/devices/*; do
        echo -e "\t$(lspci -nns ${d##*/})"
    done;
done;

在PVE下直通AMD平台的SATA接口要把PCI-E这个选项打开。
<img width="555" alt="PVE直通SATA" src="https://user-images.githubusercontent.com/32221824/155162176-f7999c94-2e7a-4c91-9d14-5084d8a88577.png">


### RTL8125B在ESXI7.0的使用经验
RTL8125B是很多低价新主板2.5G网口的标配。在6.7上有专门的驱动可以集成，但是用打包工具无法继承到7.0的安装盘中。
找到[SYSIN](https://sysin.org/blog/vmware-esxi-7-u3-nuc-usb-nvme/)大佬做的7.0 u3c安装光盘，虽然在安装界面无法识别RTL8125B，但是在使用界面发现RTL8125B是可以识别的，而且可以直通给虚拟机。
需要注意的是在硬件---》高级设计中的VMkernel.boot.disableACScheck设定值要由原来的false改为true
<img width="758" alt="ACS setting" src="https://user-images.githubusercontent.com/32221824/155163354-72946ad6-db4f-4a79-923f-0f18b3be0c6b.png">
这样就是你在ESXI上用上RTL8125B就需要先用另外一张内置/外置网卡，完成ESXI的安装，配置。