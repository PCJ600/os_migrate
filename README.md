# CentOS 7.9 系统自动迁移方法, 从单系统分区切换到双系统分区

基于Linux Emergency Mode定制initramfs, 实现自动化的重建分区和OS迁移(CentOS 7.9)

文件说明:
```
va.sgdisk # 目标系统分区表 (BIOS+GPT格式, biosboot分区 + boot分区(1G) + LVM(两个系统分区 + /data数据分区(512M) + /image分区(512M)
va.tar.xz # 将目标系统虚拟机导出OVF, 挂载VMDK根文件系统并备份
dracut-emergency.service # initrd阶段执行自定义脚本, 将目标系统备份到内存, 重建分区, 再安装目标系统
vmlinuz-iso # # 从CentOS 7.9官方ISO直接复制得到的压缩内核
initramfs-iso.img # 基于CentOS 7.9官方ISO的initrd.img定制, 用于进入紧急模式中重建分区, 迁移OS
```

迁移方法:
* CentOS7.9机器上执行gen_initrd.sh, 得到定制后的initramfs
* 执行deploy.sh, 输出文件归档到deploy, 传到源系统上执行upgrade.sh, 迁移到新系统, 同时重建分区
