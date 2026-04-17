## 查看 qcow2 镜像信息

```bash
qemu-img info debian-13-generic-amd64.qcow2
```





## 参数说明



参数|说明|备注
---|---|---
`-m 2048` | 分配 2GB 内存
`-smp 2` | 2 核 CPU
`-nographic` | 无图形界面（服务器镜像常用）