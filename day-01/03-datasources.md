```
-smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
```

## NoCloud

**Discovery configuration**
> 发现配置的目的是告诉 cloud-init 可以在哪里找到上述运行时配置。

```
ds=nocloud;s=https://10.42.42.42/configs/
```

**Configuration sources**
> 用户数据、元数据、网络配置和供应商数据可从多个可能地点之一获取，既可本地，也可以远程。





-------

这段参数：

```
-smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
```

是给 cloud-init 传递 **数据源（datasource）信息** 的，作用是：

> **告诉 cloud-init 去哪里获取初始化配置（user-data / meta-data）**

这是 Ubuntu Cloud 镜像自动初始化的关键。

------

# 一、整体拆开看

拆分后：

```
-smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
```

可以分成三部分：

------

## 第一部分：`-smbios`

这是 QEMU 的参数：

```
-smbios
```

作用是：

> **向虚拟机注入 SMBIOS 信息**

SMBIOS 是虚拟机里的“硬件信息表”，系统可以读取它。

例如虚拟机内可以看到：

- BIOS 信息
- 主板信息
- 序列号

cloud-init 会读取这里面的 **serial number** 来找配置源。

------

# 二、`type=1`

```
type=1
```

表示：

> **设置 SMBIOS System Information（系统信息）**

SMBIOS 有很多类型：

- `type=0` → BIOS 信息
- `type=1` → 系统信息
- `type=2` → 主板信息

这里 `type=1` 是告诉 QEMU：

> “给虚拟机设置系统信息”

------

# 三、`serial=...`

```
serial=ds='nocloud;s=http://10.0.2.2:8000/'
```

这是设置系统序列号：

```
serial=...
```

正常序列号可能像：

```
ABC123456
```

但 cloud-init 利用了这个字段，把它当成：

> **cloud-init datasource 配置字符串**

也就是说：

```
ds='nocloud;s=http://10.0.2.2:8000/'
```

不是普通序列号，而是 cloud-init 的指令。

------

# 四、`ds=` 是 datasource

```
ds=nocloud
```

表示：

> cloud-init 使用 **NoCloud 数据源**

NoCloud 是 cloud-init 的一种 datasource。

cloud-init 支持很多数据源：

- `ec2`
- `azure`
- `gce`
- `nocloud`

这里：

```
ds=nocloud
```

表示：

> “不要去 AWS/Azure，去指定位置读取配置”

------

# 五、`s=` 是 seedfrom

```
s=http://10.0.2.2:8000/
```

这是：

> **seedfrom**

意思是：

> **初始化配置文件的位置**

也就是告诉 cloud-init：

> 去这个地址下载：

- `meta-data`
- `user-data`

最终访问：

```
http://10.0.2.2:8000/meta-data
http://10.0.2.2:8000/user-data
```

------

# 六、为什么是 `10.0.2.2`

在 QEMU 的 user networking 模式下：

```
-netdev user
```

虚拟机默认网络：

| 地址        | 含义       |
| ----------- | ---------- |
| `10.0.2.15` | 虚拟机自己 |
| `10.0.2.2`  | 宿主机     |
| `10.0.2.3`  | DNS        |

所以：

```
http://10.0.2.2:8000/
```

表示：

> **访问宿主机上的 8000 端口**

如果宿主机运行：

```
python3 -m http.server 8000
```

虚拟机就能访问：

```
http://10.0.2.2:8000/user-data
```

------

# 七、cloud-init 实际工作流程

当 Ubuntu cloud image 启动时：

------

## 第一步：读取 SMBIOS serial

读取到：

```
ds=nocloud;s=http://10.0.2.2:8000/
```

------

## 第二步：识别 datasource = NoCloud

使用 NoCloud

------

## 第三步：访问：

```
http://10.0.2.2:8000/meta-data
http://10.0.2.2:8000/user-data
```

------

## 第四步：应用配置

例如 `user-data`：

```
#cloud-config
users:
  - name: ubuntu
    sudo: ALL=(ALL) NOPASSWD:ALL
    plain_text_passwd: ubuntu
    lock_passwd: false
```

cloud-init 自动：

- 创建用户
- 设置密码
- 配 SSH key
- 执行命令

------

# 八、为什么要这样做？

因为你用的是：

> **cloud image**

比如 Ubuntu Server 24.04 ARM64 Cloud Image 默认：

- 没有密码
- 没有用户初始化
- 没有 SSH key

必须通过 cloud-init 提供配置。

这个 `-smbios` 参数就是：

> **把 cloud-init 配置地址告诉虚拟机**

------

# 九、等价理解

你可以把：

```
-smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
```

理解成：

> “启动后，请去 `http://10.0.2.2:8000/` 找初始化配置”

------

# 十、如果不用 HTTP，也可以用本地 ISO

比如制作一个 cidata：

```
-cloud-init.iso
```

里面放：

- `user-data`
- `meta-data`

然后挂载：

```
-drive file=seed.iso,if=virtio
```

也是 NoCloud datasource。

------

# 总结一句：

```
-smbios type=1,serial=ds='nocloud;s=http://10.0.2.2:8000/'
```

作用就是：

> **通过 SMBIOS 告诉 cloud-init：去宿主机的 `http://10.0.2.2:8000/` 下载初始化配置**

这是 cloud image 自动初始化用户和密码的核心。

------

如果你想，我下一步可以帮你解释：

> **为什么 cloud-init 要求必须有 `meta-data` 和 `user-data` 两个文件，缺一个就可能启动失败**


## Ref



- <https://docs.cloud-init.io/en/latest/reference/datasources.html>
- <https://docs.cloud-init.io/en/latest/reference/datasources/nocloud.html>