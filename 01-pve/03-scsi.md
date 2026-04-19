## 03 - scsi 控制器

```bash
--scsihw virtio-scsi-single
--scsihw virtio-scsi-pci
```


二、核心区别一句话

⸻

virtio-scsi-pci

所有磁盘共享一个 SCSI 控制器

⸻

virtio-scsi-single

每个磁盘独占一个 SCSI 控制器

⸻