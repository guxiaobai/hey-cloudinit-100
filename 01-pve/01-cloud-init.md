## 01 - cloud-init

```bash
qm set 100 --ide2 local-lvm:cloudinit
```

```bash
qm set 100 --ciuser ubuntu --cipassword 123456
qm set 100 --sshkey /root/id_rsa.pub
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

## 01[已解决] - 如何在 local 里面增加 Snippets

六、需要先启用 snippets 类型

```bash
pvesm status
```

```bash
cat /etc/pve/storage.cfg
```

如果没有：

```
snippets
```

就不能用 `--cicustom`

# 十、一句话总结

```
--ciuser --cipassword
```

可以和：

```
--cicustom
```

一起使用，但：

> **如果 `--cicustom` 指定了 `user=...`，就会覆盖 `--ciuser --cipassword` 自动生成的 `user-data`**

所以：

> **`user=...` 和 `--ciuser` 不建议同时用；但 `network=...` 可以和 `--ciuser` 一起用**

------

如果你下一步想知道：

> **当 `user=...` 覆盖自动生成时，`--sshkey` 会不会也失效**

这个也是同一套优先级逻辑，我可以继续解释。