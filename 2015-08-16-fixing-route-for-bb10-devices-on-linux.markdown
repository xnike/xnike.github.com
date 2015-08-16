Title: Fixing route for BB10 devices on Linux system.
Date: 2015-08-16 11:00
Tags: BB10, BlackBerry 10, Linux

After updating my BlackBerry Q10 with name "Q10" to OS 10.3.1 I started expecting connection issue via USB cable:

```
[root@localhost ~]# ping Q10.local
PING Q10.local (169.254.141.173) 56(84) bytes of data.
^C
--- Q10.local ping statistics ---
5 packets transmitted, 0 received, 100% packet loss, time 3999ms
```

Output of "route" command shows that device was recognized by system:

```
[root@localhost ~]# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         gateway         0.0.0.0         UG    600    0        0 wlp3s0
Q10.local       linux.local     255.255.255.255 UGH   100    0        0 enp14s0u1
...
```

As a fix you can repeat  the same command used by system to create item in the kernel ip routing table (you should the same Iface you see in the route output):

```
route add -net Q10.local netmask 255.255.255.255 gw linux.local dev enp14s0u1
```

And now everything is OK:

```
[root@localhost ~]# ping Q10.local
PING Q10.local (169.254.141.173) 56(84) bytes of data.
64 bytes from Q10.local (169.254.141.173): icmp_seq=1 ttl=255 time=2.16 ms
64 bytes from Q10.local (169.254.141.173): icmp_seq=2 ttl=255 time=2.51 ms
64 bytes from Q10.local (169.254.141.173): icmp_seq=3 ttl=255 time=2.29 ms
^C
--- Q10.local ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2001ms
rtt min/avg/max/mdev = 2.164/2.324/2.512/0.153 ms
```
