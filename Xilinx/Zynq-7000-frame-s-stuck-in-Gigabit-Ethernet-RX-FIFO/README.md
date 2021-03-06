Linux have a special ICMP socket type you can use with:

```c
  socket(PF_INET, SOCK_DGRAM IPPROTO_ICMP);
```

This allows you to only send ICMP echo requests The kernel will handle it specially (match request/responses, fill in the checksum).

This only works if a special sysctl is set. By default not even root can use this kind of socket. You specify the user groups that can access it. To allow root (group 0) to use ICMP sockets, do:

```bash
 sysctl -w net.ipv4.ping_group_range="0 0"
```

Typically the user in a Linux box has gid 1000 (run the `id` command). So this will become 

```bash
 sysctl -w net.ipv4.ping_group_range="1000 1000"
```

You might want to set this up in /etc/sysctl.conf as


```bash
...
net.ipv4.ping_group_range = 1000 1000
...
```
