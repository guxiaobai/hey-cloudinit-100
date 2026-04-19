## 01 PVE

* debian-13-template
* ubuntu-2404-tempalte

```bash
qm create 9000 \
  --name ubuntu-2404-tempalte \
  --machine q35 \
  --memory 2048 \
  --cores 1 \
  --cpu host \
  --net0 virtio,bridge=vmbr0 \
  --scsihw virtio-scsi-single \
  --serial0 socket \
  --vga serial0 \
  --agent enabled=1
```

```bash
--bios
```

```bash

qm importdisk 9000 /var/lib/vz/template/qcow2/noble-server-cloudimg-amd64.img local-lvm
# qm importdisk 9000 debian-13-generic-amd64.qcow2 local-lvm
qm set 9000 --scsi0 local-lvm:vm-9000-disk-0
qm set 9000 --ide2 local-lvm:cloudinit
qm set 9000 --boot order=scsi0
qm template 9000
```

```bash
qm clone 9000 100 --full 1 --name 'vm01'
```

所以最佳实践是：

> **模板小磁盘 + 克隆后 `qm resize` + cloud-init 自动扩容**

```bash
qm resize 100 scsi0 20G
qm set 100 --ipconfig0 ip=192.168.100.50/24,gw=192.168.100.2
```




## qm

命令|说明
---|---
`help` | 帮助
`list` ｜ 列表
`destroy` | 销毁
`create` | 创建
`start` | 启动
`template` | 模版




# 虚拟机的 Machine 类型

参数|说明
---|---
`--machine` | i440fx / q35

因为 Proxmox 约定：

- `scsi0` → 系统盘
- `ide2` → cloud-init 盘


---

Proxmox 的存储标识格式

```bash
pvesm status
cat /etc/pve/storage.cfg
```


## Ref

- <https://pve.proxmox.com/pve-docs/qm.1.html>