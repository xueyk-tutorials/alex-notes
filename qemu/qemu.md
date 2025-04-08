# qemu

## 安装

## 基础使用

取消鼠标捕获：Ctrl+Alt+G



# 实例

## 知乎1

```bash
qemu-img create -f qcow2 E:\D-qemu\ubuntu_arm64.img 20G

qemu-system-aarch64.exe -m 8192 -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios E:\D-qemu\QEMU_EFI.fd -net nic -net tap,ifname=tap -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=E:\D-qemu\ubuntu-20.04-live-server-arm64.iso,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom -drive if=none,file=E:\D-qemu\ubuntu_arm64.img,id=hd0 -device virtio-blk-device,drive=hd0


```



## deepseek（不行）

```bash
qemu-system-aarch64 -M virt -cpu cortex-a72 -smp 4 -m 8G -bios E:\D-qemu\QEMU_EFI.fd -device virtio-gpu-pci -device usb-ehci,id=usb -device usb-kbd -device usb-mouse -drive file=E:\D-qemu\arm64vm.qcow2,if=virtio,format=qcow2 -cdrom E:\D-qemu\ubuntu-20.04-live-server-arm64.iso -boot menu=on -netdev user,id=net0 -device virtio-net-device,netdev=net0


qemu-system-aarch64 -M virt -cpu cortex-a72 -smp 4 -m 4G -bios E:\D-qemu\QEMU_EFI.fd -device virtio-gpu-pci  -device usb-ehci,id=usb -device usb-kbd -device usb-mouse -drive file=E:\D-qemu\arm64vm.qcow2,if=virtio,format=qcow2 -netdev user,id=net0 -device virtio-net-device,netdev=net0
```



## windows-amd64主机平台安装arm64环境

### 创建一个磁盘

```bash
qemu-img create -f qcow2 ubuntu_arm64.img 16G
```

### 安装镜像

```bash
qemu-img create -f qcow2 ubuntu_arm64.img 16G

qemu-system-aarch64 -m 4G -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios E:\D-qemu\QEMU_EFI.fd -net nic -net tap,ifname=tap -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=E:\D-qemu\ubuntu-20.04-live-server-arm64.iso,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom -drive if=none,file=E:\D-qemu\ubuntu_arm64.img,id=hd0 -device virtio-blk-device,drive=hd0

qemu-system-aarch64 -m 4000 -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios E:\D-qemu\QEMU_EFI.fd -net nic -net tap,ifname=tap -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=E:\D-qemu\ubuntu_arm64.img,id=hd0 -device virtio-blk-device,drive=hd0

```

无网卡

```bash
qemu-system-aarch64 -m 4G -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios E:\D-qemu\QEMU_EFI.fd -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=E:\D-qemu\ubuntu-20.04-live-server-arm64.iso,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom -drive if=none,file=E:\D-qemu\ubuntu_arm64.img,id=hd0 -device virtio-blk-device,drive=hd0
```





```bash
qemu-system-aarch64 -m 4G -cpu cortex-a72 -smp 4 -M virt -bios E:\D-qemu\QEMU_EFI.fd -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -cdrom E:\D-qemu\ubuntu_arm64 -hda E:\D-qemu\ubuntu-20.04-live-server-arm64.iso
```

参数含义如下：

-m 4000 表示分配给虚拟机的内存最大4000MB

-cpu cortex-a72 指定CPU类型，还可以选择cortex-a53、cortex-a57等smp 4,cores=4,threads=1,sockets=1 指定虚拟机最大使用的CPU核心数等

-M virt 指定虚拟机类型为virt，具体支持的类型可以使用 qemu-system-aarch64 -M help 查看-bios Z:QEMU EFI.fd 指定UEFI固件文件

-net nic,model=pcnet 启用网络功能
-device nec-usb-xhci 

-device usb-kbd 

-device usb-mouse 启用USB鼠标等设备

-device VGA 启用VGA视图，对于图形化的Linux这条很重要!

-drive if=none,file=Z:\uos.iso,id=cdrom,media=cdrom 指定光驱使用镜像文件

-device virtio-scsi-device -device scsi-cd,

-drive=cdrom 指定光驱硬件类型

-drive if=none,file=Z:\uos.img指定硬盘镜像文件



### 启动

```bash
qemu-system-aarch64 -m 4G -cpu cortex-a72 -smp 4 -M virt -bios E:\D-qemu\QEMU_EFI.fd -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=E:\D-qemu\ubuntu_arm64,id=hd0 -device virtio-blk-device,drive=hd0
```





```bash
qemu-system-aarch64 -m 4G -cpu cortex-a72 -smp 4 -M virt -bios E:\D-qemu\QEMU_EFI.fd -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=E:\D-qemu\ubuntu_arm64,id=hd0 -net nic -net tap,ifname=tap0
```



有网卡启动



# ubuntu主机





```
curl -L https://github.com/multiarch/qemu-user-static/releases/download/v7.2.0-1/qemu-aarch64-static -o ~/Desktop/qemu/qemu-aarch64-static
```





