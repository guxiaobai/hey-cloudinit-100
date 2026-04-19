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