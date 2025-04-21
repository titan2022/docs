# Setting up a SOCKS proxy

Oftentimes you might need to provide Internet to a computer you can control via SSH but which doesn't have its own Internet.

## Creating the proxy

<https://unix.stackexchange.com/questions/116867/serve-internet-to-remote-machine-via-ssh-session>

Follow these instructions on the machine with internet access.

You might need to enable the SSH daemon. On openSUSE this is:

```bash
sudo systemctl start sshd
```

Then you can create the proxy. This is kinda a terrible hack but it works:

```bash
ssh -D 1080 localhost -t ssh -R 1080:localhost:1080 pi@opi_ip_address
```

## Set up the remote

<https://superuser.com/questions/1401585/how-to-force-all-linux-apps-to-use-socks-proxy#1402071>

Follow these steps on the remote machine.

Install redsocks:

```bash
sudo apt install redsocks
```

This probably doesn't work since the remote doesn't have internet access. Instead, you can download the file on the machine that has internet and then transfer it to the remote.

```bash
wget http://ports.ubuntu.com/pool/universe/r/redsocks/redsocks_0.5-2build4_arm64.deb http://ports.ubuntu.com/pool/main/libe/libevent/libevent-core-2.1-7t64_2.1.12-stable-9ubuntu2_arm64.deb # for 24.04 arm64
ssh pi@opi_ip_address mkdir -p ~/Downloads/aptpackages
scp redsocks_0.5-2build4_arm64.deb libevent-core-2.1-7t64_2.1.12-stable-9ubuntu2_arm64.deb pi@opi_ip_address:~/Downloads/
ssh pi@opi_ip_address
    # inside the SSH session:
    sudo dpkg -i ~/Downloads/aptpackages/libevent-core-2.1-7t64_2.1.12-stable-9ubuntu2_arm64.deb ~/Downloads/aptpackages/redsocks_0.5-2build4_arm64.deb
```

Make the redsocks conf file:

```bash
cat <<EOF >redsocks.conf
base {
    log_debug = on;
    log_info = on;
    log = "stderr";
    daemon = off;
    redirector = iptables;
}

redsocks {
    local_ip = 127.0.0.1;
    local_port = 12345;

    ip = localhost;
    port = 1080;
    type = socks5;
    // known types: socks4, socks5, http-connect, http-relay

    // login = login;
    // password = password;
}
EOF
```

Run redsocks:

```bash
sudo killall redsocks
sudo redsocks -c ./redsocks.conf
```

You probably want to make it outlast the bash session, so:

```bash
nohup sudo redsocks -c ./redsocks.conf &
```

No output will be printed to the terminal, so use it without nohup first to make sure it works.

Set up `iptables` (which controls the Linux kernel firewall):

```bash
sudo iptables -t nat -N REDSOCKS

sudo iptables -t nat -A REDSOCKS -d 0.0.0.0/8 -j RETURN
sudo iptables -t nat -A REDSOCKS -d 10.0.0.0/8 -j RETURN
sudo iptables -t nat -A REDSOCKS -d 127.0.0.0/8 -j RETURN
sudo iptables -t nat -A REDSOCKS -d 169.254.0.0/16 -j RETURN
sudo iptables -t nat -A REDSOCKS -d 172.16.0.0/12 -j RETURN
sudo iptables -t nat -A REDSOCKS -d 192.168.0.0/16 -j RETURN
sudo iptables -t nat -A REDSOCKS -d 224.0.0.0/4 -j RETURN
sudo iptables -t nat -A REDSOCKS -d 240.0.0.0/4 -j RETURN
    
sudo iptables -t nat -A REDSOCKS -p tcp -j REDIRECT --to-ports 12345
    
sudo iptables -t nat -A OUTPUT -p tcp --dport 443 -j REDSOCKS
sudo iptables -t nat -A OUTPUT -p tcp --dport 80 -j REDSOCKS
    
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDSOCKS
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDSOCKS
```

It will be reset on reboot, which is probably what you want.

TODO: Get DNS over SOCKS

