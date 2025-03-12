# Network sharing

Say you have a laptop with Internet connection via Wi-Fi and a Pi with Ethernet connection to the laptop. You might need to access the internet from the Pi.

To do it this way, we have an Ethernet cable wired directly from the laptop to the Pi.

On the laptop, run

```bash
nmcli device
```

to see the network connections. Look at the device that has Ethernet. On my laptop it is `eth0`. 

We need to run a command of the form
```bash
nmcli connection add con-name <name> type ethernet ifname <iface> ipv4.method shared ipv6.method ignore
```

For me, I'll use
```bash
nmcli connection add con-name sharedinternet type ethernet ifname eth0 ipv4.method shared ipv6.method ignore
```

Then, we can activate the connection:
```bash
nmcli connection up sharedinternet
```

By default, all IP addresses are allocated behind `10.42.0.1/24` (IP addresses between 10.42.0.0 and 10.42.0.255). Therefore, you could run `nmap 10.42.0.1/24` or use Angry IP Scanner or another scanner in order to find the Pi's IP address.

## References

* <https://ubuntu.com/core/docs/networkmanager/configure-shared-connections>
* <https://fedoramagazine.org/internet-connection-sharing-networkmanager/>
