

## WG-EASY

https://github.com/wg-easy/wg-easy


below config taken from here:

https://docs.techdox.nz/wgeasy/#docker-compose-configuration

comments help:

```yml
services:
  wg-easy:
    image: ghcr.io/wg-easy/wg-easy  # The Docker image to use.
    container_name: wg-easy         # Name of the container.
    environment:                    # Environment variables to configure the instance.
      - LANG=en                     # Language settings.
      - WG_HOST=<Your IP/Domain>    # Public IP or domain name where WG-Easy is accessible.
      - PORT=51821                  # Port for the web interface.
      - WG_PORT=51820               # WireGuard port for VPN traffic.
    volumes:
      - ./wg-easy/:/etc/wireguard   # Volume mapping for WireGuard configuration files.
    ports:
      - "51820:51820/udp"           # UDP port used by WireGuard.
      - "51821:51821/tcp"           # TCP port for accessing the web interface.
    cap_add:                        # Capabilities required for managing networking features.
      - NET_ADMIN
      - SYS_MODULE
    sysctls:                        # Kernel parameters that need to be set for WireGuard.
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv4.ip_forward=1
    restart: unless-stopped         # Ensures the container restarts automatically unless manually stopped.
    ```
```





## port forwarding

in order for all this to work, your router has to know how to forward ports

read the other tutorial on how to access your router


my router is so shit it forces me to download an app to do port forwarding.  se here's a mobile screenshot

![Router Configure Port Forwarding](images/router-port-forwarding.jpg)

we only need to forward UDP ports because thats what the wireguard VPN uses.  the 51820 you'll notice aligns with the UDP port in your `docker-compose.yml`



## WINDOWS INSTALL

lets set it up on windows

https://github.com/WireGuard/wireguard-windows


https://www.wireguard.com/install/

download and install

after installing then generate a conf file on wg-easy and import it



lets make the device running the config itself a client

pictures and screenshots

now lets make the server a client



sudo mkdir /etc/wireguard

sudo nano /etc/wireguard/wgo.conf

```conf

[Interface]
etc

[Peer]
etc
```

MAKE SURE THAT YOU REMOVE THE DNS = ENTRY FROM THE CLIENT THAT IS RUNNING ON THE SERVER

run this

```bash
sudo apt update
sudo apt install wireguard
```
you might also need

```
sudo apt install resolvconf
```

then this:

```
sudo wg-quick up wg0
```

should be this:

```
eagleson@eagleson-OptiPlex-7040:~/home-server/wg-easy$ sudo wg-quick up wg0
Warning: `/etc/wireguard/wg0.conf' is world accessible
[#] ip link add wg0 type wireguard
[#] wg setconf wg0 /dev/fd/63
[#] ip -4 address add 10.8.0.3/24 dev wg0
[#] ip link set mtu 1420 up dev wg0
```


hey dns?  lets play traceroute

make sure you can ping www.google.com when the wg0 interface is up


## UPDATING

https://github.com/WeeJeWel/wg-easy#updating

## UFW firewall allow ssh on the VPN

sudo ufw allow from 10.8.0.0/24 to any port 22 proto tcp



## Running it every time on reboot

```bash
sudo crontab -e
```

Select 1 for `nano` or whatever you like

Add this line:

```
@reboot wg-quick up wg0
```


TODO: 
## Firewall

sudo ufw allow from 10.8.0.0/24 to any port 22 proto tcp