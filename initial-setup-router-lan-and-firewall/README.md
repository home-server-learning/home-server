
# Router Configuration

## Core Concept Introduction

As a prerequisite to most things, you are going to want to make sure that you can access the admin interface of your router and know how to navigate to manage a couple of the basic things.  

If you don't know anything about IP addresses and subnets, [read this before going any further](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html).  It's not that long.  

You don't have to understand all of it, just skim it enough to familiarize yourself with the general idea.  You can look up specific details later as you need them.  

The main things you should really understand to be able to both set this stuff up, but also have some general idea how to troubleshoot it if things don't work on the first try are:

* What an IP address is (specifically IPv4 address)

* What a default gateway is (basically it means your router -- for our purposes it's whatever the device between your machine and the internet is)

* What a subnet is. [Great video here](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html).

* What a static IP is and the difference between that and [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol). (Basically it just means your device will get the same IP every time instead of a random one so that you can have a permanent address to always refer to it.)

* What [port forwarding](https://en.wikipedia.org/wiki/Port_forwarding) is. (WE use this to tell devices sending messages into your network from the outside internet which specific device in your house is running the service they want. For example "oh you're requesting data on port 12345?  Yeah that request should be forwarded to the computer in my basement running the Minecraft server)

* The difference between a public IP address (e.g. the IP address of your router that the wider internet sees, the one you see if you go to [whatsmyip.org](https://www.whatsmyip.org/)) and the internal (LAN) IP addresses of the devcies in your house.  The public IP must be unique across the entire world, but the internal IPs can be the same as devices in someone else house.  The translation between public IP to the internal private ones is called [network address translation](https://en.wikipedia.org/wiki/Network_address_translation) or `NAT`.

* The difference between [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) and [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol).  Understanding the full details are absolutely not required, just the first couple paragraphs of each is fine.  This will be relevant if you are planning to set up a [Wireguard VPN](https://en.wikipedia.org/wiki/WireGuard).  Wireguard is build on top of UDP.

There's always more, but I think those are the main fundamentals.  If you have a good idea what all those are you're in great shape.  

## Accessing Your Router Admin Interface

Before you can try to log into it you need to know its internal (LAN) IP address.  

**IMPORTANT**: If you are using the default modem & router (or even combo of both) that was provided by your ISP (Internet Service Provider) it's not unusual these days for them to give you a device that's pretty locked down or limited to what you can configure.  

In my case for example, I can access to router's admin interface, but they (name and shame [Rogers](https://www.rogers.com/)) have locked or blocked a lot fo the router's built in features and force you to use the app.  For example I can only do port forwarding on their mobile app.

If that is the case for you as well, then hopefully it doesn't fully block you from doing these things.  Even though the ISP makes it very inconvenient, I still (at least for now) have the ability to use the app to configure these things.

If you can't, or you just don't want to deal with that kind of limited control the solution is to put the router into [bridge mode](https://en.wikipedia.org/wiki/Bridge_router) and buy your own router where you have full control.  If you are going to go that route, make sure you pick a line where you have complete control and the company that produces them doesn't have visibility into your network.  Most folks in the home networking space recommend [Ubiquiti UniFi](https://www.ui.com/).

Alright back to accessing your router (if you can).

If you're on Windows then you can Powershell and run `ipconfig`.

Or better yet use the [Git bash terminal](https://git-scm.com/downloads) and run the same commands as Mac/Linux.

On bash, Mac or Linux type `ifconfig`.

If you are on Linux and get an error saying that the tool is not installed, run this command (the update is because it'll make sure you're looking for the latest version when you install):

If `sudo` gives you an issue see the [Initial Hardware Setup Guide](initial-setup-hardware/README.md) which covers what it is.


```bash
sudo apt update

sudo apt install net-tools
```

Now you can:

```bash
ifconfig
```
Below is an example of my using `ipconfig` on Windows, but the `ifconfig` output will look very similar.  

```bash
alexe@alex-zephyrus MINGW64 ~
$ ipconfig

Windows IP Configuration

Wireless LAN adapter Wi-Fi:

   Connection-specific DNS Suffix  . : phub.net.cable.rogers.com
   IPv6 Address. . . . . . . . . . . : 2607:fea8:5c96:a700::351f
   IPv6 Address. . . . . . . . . . . : 2607:fea8:5c96:a700:7dc3:97e6:c388:a25c
   Temporary IPv6 Address. . . . . . : 2607:fea8:5c96:a700:14eb:73b8:d4f3:f6c4
   Link-local IPv6 Address . . . . . : fe80::805d:6aec:4362:9438%3
   IPv4 Address. . . . . . . . . . . : 10.0.0.188
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : fe80::cef3:c8ff:fe68:18e7%3
                                       10.0.0.1
```

What you're looking for will depend on how you're connected.  if you're connected over wifi look for something like `Wireless LAN adapter Wi-Fi:` on Windows, or might be `wlan0` on other systems.  

Similar if you're hard wired on this machine to ethernet, look for `ethernet`, or `eth0`.  

Once you find it the IP address you're looking for is the one called `Default Gateway`.  This is a term that refers to the device on your network which basically acts as a gateway to the internet, and for most people that will be the router.

Copy the IP address of your router (the default gateway IPV4 address which is the one in the format X.X.X.X, not the longer X::X::X::X::X one) somewhere, ideally to your password/credential manager.  Create a record for your router where you'll also store the username/password too when we get there.  

Often the gateway address will be something like 10.0.0.1 or 192.168.0.1 or 172.0.0.1.  Those are common internal IP subnets for popular routers.  Yours may be different though.  

Enter the IP into your browser address bar and hit enter and hopefully you'll be hit by a login splash page along these lines.  Every router is different though.

TODO:

![Router Login Page](images/router-login-page.png)

If you already know your username and password, great!  If not, it's usually some combination of "admin" by default and even "password" for the password.  

Your best bet is to lookup the model number of the device on Google and search for "default password" you should be able to find the default username and password for that device.  You can change it after if you like.  You should, and then store those credentials in your credential manager so they are easy to access.

Once you're in you'll want to look for something that shows a list of "connected devices".  Different brands will call it different things.  

That should allow you to see everything connected to your router (take a moment to sanity check that you know what everything is.  Keep in mind that IoT devices means anything in your house might have connected, a lot of stuff you may not think about like smart bulbs, appliances, robot vacuums etc.  Just because you see something you don't recognize doesn't mean it's malicious).

Although it could be -- it's always a good idea to be aware what devices are connected to your home network.  This is where you go to get that information.

To start with in the list of connected devices and most importantly is going to be your home server.  In my case, the Dell Optiplex.  Fortunately it showed up there for me by name, hopefully you are able to identify your machine in a similar way.  

What you want to do is disable [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) and give the device a static IP address for within your network.  Most of the time just using the one it's already been randomly assigned is fine unless you have reason to use another.  

The goal here really is to keep it from changing.

Make sure to write down that IP address in your credential manager.  You shoudl create a new entry for everything related to your home server.  Put the IP address you've just assigned to it in the same document as the Ubuntu username and password.  

You can look it up easy anytime, it'll be a very important one to remember.

As you can see as example below, the internal (LAN) IP address of my home server is 10.0.0.48.  

![Set Static IP in Router Settings](images/router-static-ip.png)





# Firewall

Assuming you're following this guide and installed Ubuntu on your server, it comes pre-installed with a firewall called `ufw` a.k.a [Uncomplicated Firewall](https://help.ubuntu.com/community/UFW).

Making sure you're familiar with it, how to configure it and how to enable/disable it in advance can help prevent headache down the road, since a firewall blocking connections is not always the easiest thing to identify as the cause of something not working (but fortunately `ufw` does make it very easy to fix once you do eventually ask if maybe it's the firewall).

If you're not familiar with a [firewall](https://en.wikipedia.org/wiki/Firewall_(computing)), it's program that monitors network traffic and decides whether or not to allow connections based on a set of rules.

You can check UFW's status with:

```bash
sudo systemctl status ufw
```

And should see output like:

```
eagleson@eagleson-OptiPlex-7040:~$ sudo systemctl status ufw

● ufw.service - Uncomplicated firewall
     Loaded: loaded (/usr/lib/systemd/system/ufw.service; enabled; preset: enabled)
     Active: active (exited) since Sun 2025-03-30 21:32:33 EDT; 4 days ago
       Docs: man:ufw(8)
   Main PID: 469 (code=exited, status=0/SUCCESS)
        CPU: 59ms
```

If it's disabled (and you're okay with that) then nothing need be done, but unless you are absolutely sure you don't want it, you should enable it.  It's very easy to configure to allow the services you use, while blocking everything else.

You can disable it with:

```bash
sudo systemctl ufw disable
```

And enable it with:

```bash
sudo systemctl ufw enable
```

Once you have services running you'll be able to permit them through with:

```bash
sudo ufw allow SOME_SERVICE_DETAILS
```

Where `SOME_SERVICE_DETAILS` will be anything form a name of a program, to a specific IP address and protocol to allow through.

The above commands are all you really need to know for the basics.  We'll get a bit deeper into it with some of the specific tutorials.  

If you want to learn more about it now, [Digital Ocean has a great guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu).