## 01 - cloud-init

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

**自定义**

```bash
qm set 100 --cicustom "user=local:snippets/user-data.yaml"
# /var/lib/vz/snippets/user-data.yaml
```