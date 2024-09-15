Linux系统安装启动U盘制作方法
=======================================

**步骤**

1. 将U盘插入正在运行的Windows计算机。

2. 以管理员身份打开“命令提示符”（CMD）窗口。

3. 输入 diskpart。diskpart 是一个Windows命令行工具，用于管理磁盘分区和卷。

4. 输入 list disk。list disk 命令将显示计算机上的所有磁盘，需要确认好输入U盘对应的驱动器编号。

5. 输入 select disk <X>，其中 X 是 U盘对应的驱动器编号。

6. 输入 clean。该命令删除U盘中的所有数据，操作前请备份好U盘中数据。

7. 输入 create partition primary size=10240。创建新的主分区，大小为10G，可不指定size，默认使用全部空间，注意如果选择格式化的系统为fat32，分区大小不能超过10G。

8. 输入 select partition 1。选择刚创建的分区。

9. 输入 format fs=fat32 label="OC9-2" quick 或 format fs=ntfs label="OC9-2" quick。对分区进行格式化，如目标机器支持统一可扩展固件接口 (UEFI)，则应将 U 盘格式化为fat32，而非ntfs。label后面为文件系统名称。

10. 输入 active。将选定的分区设置为活动分区。

11. 输入 exit。退出 DiskPart 环境。

12. 将启动镜像IOS文件装载后，拷贝至 U 盘的根目录。

13. 修改EFT/BOOT/grub.cfg或boot/grub2/grub.cfg，不同启动系统（UEFI或传统BIOS）会选择不同文件启动，不确定的话可同时进行修改。从文件中找到所有linuxefi或linux命令，将该命令的入参inst.stage2=hd:LABEL:xxx修改为inst.stage2=hd:LABEL:OC9-2，同时在命令最后加入参数inst.repo=hd:LABEL:OC9-2（若使用外网配置安装源则不需要），其中“OC9-2”就是第9步设置的文件系统名称（确保正确的话可将其他文件所有带xxx的文件均进行修改）。

14. 完成启动盘制作，通过USB方式插入目标机器进行系统安装


**参考**

1. `创建可启动的 U 盘 <https://learn.microsoft.com/zh-cn/windows-server-essentials/install/create-a-bootable-usb-flash-driveL>`_
2. `opencloudos系统镜像下载 <https://mirror.iscas.ac.cn/opencloudos/>`_

