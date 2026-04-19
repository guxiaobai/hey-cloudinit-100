# Cloud-init

```bash
qemu-system-x86_64 \
    -enable-kvm \
    -cpu host \
    -m 1024 \
    -nographic \
    -drive file=noble-server-cloudimg-amd64.img,if=virtio,format=qcow2 \
    -netdev user,id=net0,hostfwd=tcp::2222-:22 \
    -device virtio-net-pci,netdev=net0 \
    -smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
```

## 如果你还要 SSH 转发（推荐）

```
-netdev user,id=net0,hostfwd=tcp::2222-:22
```

```
ssh ubuntu@127.0.0.1 -p 2222
```


## qemu-system-x86_64: -nographic cannot be used with -daemonize


## Ref

- <https://docs.cloud-init.io/en/latest/index.html>
- [Get images — Virtual Machine Image Guide documentation](https://docs.openstack.org/image-guide/obtain-images.html)
- [在 PVE 中定制和使用模板虚拟机 | 迷子笔记](https://moenew.us/proxmox-cloudinit-vm-template.html) -- 重要
- [Cloud-Init 初始化自己的云镜像 - 正汰的学习笔记](https://blog.hz2016.com/2025/03/cloud-init-%E5%88%9D%E5%A7%8B%E5%8C%96%E8%87%AA%E5%B7%B1%E7%9A%84%E4%BA%91%E9%95%9C%E5%83%8F/)
- <https://github.com/0voice/kernel_awsome_feature/tree/main>
- <https://wokron.github.io/>
- <https://documentation.ubuntu.com/public-images/>