## x86 for Ubuntu


|本期版本|上期版本
|:---:|:---:
`Fri Apr 17 10:00:15 CST 2026` | -


```bash
sudo apt install qemu-system-x86

# https://www.qemu.org/download/#linux
sudo apt-get install qemu-system
```



```bash
wcurl https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img
```

```bash
python3 -m http.server --directory .
```



```bash
qemu-system-x86_64                                            \
    -net nic                                                    \
    -net user                                                   \
    -machine accel=kvm:tcg                                      \
    -m 512                                                      \
    -nographic                                                  \
    -hda noble-server-cloudimg-amd64.img                        \
    -smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
```



## Ref

- <https://docs.cloud-init.io/en/latest/tutorial/qemu.html>