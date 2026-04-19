# 02 - Agent

```bash
apt install qemu-guest-agent
systemctl enable --now qemu-guest-agent
```

```bash
qm agent 9000 ping
```