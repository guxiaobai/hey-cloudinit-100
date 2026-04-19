## 05 - qcow2 镜像

```bash
qemu-img info debian-13-generic-amd64.qcow2
```

```bash
qemu-img resize noble-server-cloudimg-amd64.img 20G
```

直接扩容：

```bash
qm resize 9000 scsi0 +20G
```

```
df -h
lsblk
```

```
sudo growpart /dev/sda 1
sudo resize2fs /dev/sda1
```
---

