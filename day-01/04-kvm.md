# 五、确保 KVM 可用

> qemu-system-x86_64: failed to initialize kvm: Permission denied

先检查：

```
ls /dev/kvm
```

如果存在：

```
/dev/kvm
```

说明 KVM 可用。

------

## 查看 CPU 支持：

Intel：

```
egrep -c '(vmx|svm)' /proc/cpuinfo
```

如果结果大于 `0`，说明支持硬件虚拟化。


# 六、如果权限不足

可能报：

```
Could not access KVM kernel module
```

需要：

```
sudo usermod -aG kvm $USER
```

然后重新登录。