## 01 PVE


```bash
qm create 9000 \
  --name debian-13-template \
  --machine q35 \
  --memory 2048 \
  --cores 2 \
  --cpu host \
  --net0 virtio,bridge=vmbr0 \
  --scsihw virtio-scsi-single \
  --serial0 socket \
  --vga serial0 \
  --agent enabled=1
```

```bash
qm importdisk 9000 debian-13-generic-amd64.qcow2 local-lvm
qm set 9000 --scsi0 local-lvm:vm-9000-disk-0
qm set 9000 --ide2 local-lvm:cloudinit
qm set 9000 --boot order=scsi0
qm template 9000
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