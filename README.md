# Cloud-init

```bash
qemu-system-x86_64 \
    -machine accel=kvm \
    -cpu host \
    -m 1024 \
    -nographic \
    -drive file=noble-server-cloudimg-amd64.img,if=virtio,format=qcow2 \
    -netdev user,id=net0 \
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