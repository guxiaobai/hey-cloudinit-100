## 00 - 查看 qcow2 镜像信息

```bash
qemu-img info debian-13-generic-amd64.qcow2
```

## 01 - 查看配置文件

```bash
cat /etc/pve/qemu-server/100.conf
qm config 100
```

## 02 - Cloud-init

```bash
qm set 100 --ide2 local-lvm:cloudinit
```

```bash
qm set 100 --ciuser ubuntu --cipassword 123456
qm set 100 --sshkey ~/.ssh/id_rsa.pub
qm set 100 --ipconfig0 ip=192.168.100.50/24,gw=192.168.100.2
```

```bash
qm cloudinit dump 100
```
----

**自定义**

```bash
qm set 100 --cicustom "user=local:snippets/user-data.yaml"
# /var/lib/vz/snippets/user-data.yaml
```

## QEMU Guest Agent

```bash
qm set 100 --agent enabled=1
apt install qemu-guest-agent
systemctl enable --now qemu-guest-agent
```




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


---
```bash
qm start
```