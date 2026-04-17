```bash
   35  qm set 9000 --agent enabled=1
   36  qm template 9000
```

# 二、如果你在 Proxmox VE 中运行 qcow2

你之前在用 Proxmox VE，推荐这样：

------

## 1. 创建虚拟机（不要加磁盘）

例如创建 VMID 为 100：

```bash
# qm create 100 --name "ubuntu-2404-cloudinit-template" --memory 1024 --cores 1 --net0 virtio,bridge=vmbr0
qm create 100 --name debian13 --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0
```

------

## 2. 导入 qcow2 镜像

假设镜像在：

```
/root/debian-13-generic-amd64.qcow2
```

导入到存储：

```bash
# qm importdisk 100 /var/lib/vz/template/qcow2/noble-server-cloudimg-amd64.img local-lvm
qm importdisk 100 /root/debian-13-generic-amd64.qcow2 local-lvm
```

------

## 3. 挂载磁盘

导入后会生成类似：

```bash
# unused0: successfully imported disk 'local-lvm:vm-100-disk-0'
unused0: local-lvm:vm-100-disk-0
```

挂载：

```bash
# qm set 100 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-100-disk-0
qm set 100 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-100-disk-0
```

------

## 4. 设置启动盘

```bash
# qm set 100 --boot c --bootdisk scsi0
qm set 100 --boot c --bootdisk scsi0
```

------

## 5. 如果镜像是 cloud image，添加串口和 cloud-init

> @TODO

```bash
# qm set 100 --serial0 socket --vga serial0
qm set 100 --serial0 socket --vga serial0
```

然后启动：

```
qm start 100
```

# 三、如果是 cloud image，要注意默认账号

像 `debian-13-generic-amd64.qcow2` 这种通常是 **Cloud Image**，默认不能直接登录，需要配合 cloud-init 设置账号密码。

在 Proxmox 添加 cloud-init：

```bash
# qm set 100 --ide2 local-lvm:cloudinit
qm set 100 --ide2 local-lvm:cloudinit
```

设置用户名密码：

```
qm set 100 --ciuser debian --cipassword 123456
```

然后启动。


```
qm set 100 --ipconfig0 ip=192.168.100.50/24,gw=192.168.100.2
```



```bash
qm set 100 --agent enabled=1
apt install qemu-guest-agent
systemctl enable --now qemu-guest-agent