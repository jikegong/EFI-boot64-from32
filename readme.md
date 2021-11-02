# 从32位EFI启动64位linux

> 在基于z3735f的电脑棒、机顶盒等设备，安装ubuntu20.10试验成功。


### 步骤
1、以Fat32格式格式化U盘

2、使用 [rufus](https://github.com/pbatard/rufus) 烧录x64的linux iso镜像到U盘

3、打开U盘，复制本目录下所有`.efi`文件到U盘的`/EFI/boot/`目录下

4、用该U盘启动，进入live模式，打开disks磁盘管理器，找到并记住计算机的硬盘Device路径，如`/dev/mmcblk1p2`

5、安装linux后重启，拔掉U盘

6、重启应该不成功，此刻会进入grub界面，逐条输入如下内容：


```
set root=(hd0,gtp2)
linux /boot/vmlinuz-5.11.0-16-generic root=/dev/mmcblk1p2
initrd /boot/initrd.img-5.11.0-16-generic
boot
```

注意，上面的`(hd0,gtp2)`和`/dev/mmcblk1p2`要换成你自己的环境。如果是安装ubuntu，基本上`(hd0,gtp2)`不用变。

7、上面的命令输入后，应该能启动ubuntu。

8、在shell里执行：

```
sudo apt-get update
sudo apt install grub-efi-amd64 grub-efi-amd64-bin grub-efi-amd64-signed
sudo apt install grub-efi-ia32-bin grub-efi-ia32 grub-common grub2-common
sudo grub-install --target=i386-efi /dev/mmcblk1p2 --efi-directory=/boot/efi/ --boot-directory=/boot/
sudo grub-mkconfig -o /boot/grub/grub.cfg
sudo update-grub
sudo update-grub2
```

同样，根据你的情况，替换`/dev/mmcblk1p2`
