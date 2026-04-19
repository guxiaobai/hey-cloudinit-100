# 02 - Agent

```bash
qm set 100 --agent enabled=1
apt install qemu-guest-agent
systemctl enable --now qemu-guest-agent
```