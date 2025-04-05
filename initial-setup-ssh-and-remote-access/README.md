Before we continue further with setting up the actual server machine, we should switch gears and do a little bit of setup on another machine.

I'm presuming you have a computer or laptop that you use as a daily driver that is different from the server.  We're going to set up tha to be able to remotely sign into the 

### SSH into this thing


```bash
sudo apt update && sudo apt upgrade 
```

Check to see if the SSH client is installed.

```bash
ssh
```

Presuming it's installed correctly you will see something like this:

```bash
eagleson@eagleson-OptiPlex-7040:~$ ssh -v
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface] [-b bind_address]
           [-c cipher_spec] [-D [bind_address:]port] [-E log_file]
           [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file]
           [-J destination] [-L address] [-l login_name] [-m mac_spec]
           [-O ctl_cmd] [-o option] [-P tag] [-p port] [-R address]
           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
           destination [command [argument ...]]
       ssh [-Q query_option]
```

If you get output like that we can see that's it's there and ready to use.

Now we need to see if the SSH server is installed.

```bash
ssh -V
```

Output:

```
eagleson@eagleson-OptiPlex-7040:~$ ssh -V

OpenSSH_9.6p1 Ubuntu-3ubuntu13.9, OpenSSL 3.0.13 30 Jan 2024
```

If it's not recognized, then you can install it with the following command:

```bash
sudo apt install openssh-server
```

This is the machine that will be listening to incoming connections from other machines, hence being the "server" in this context.  

The ones connecting to it would install the "client".

Next check that it's actually running:

```bash
sudo systemctl status ssh
```

You should get output that looks something like this:

```
eagleson@eagleson-OptiPlex-7040:~$ sudo systemctl status ssh

○ ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: inactive (dead) since Thu 2025-04-03 21:39:24 EDT; 39min ago
   Duration: 4d 6min 48.708s
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 1629 (code=exited, status=0/SUCCESS)
        CPU: 513ms
```

If you don't, you can start it with:

```bash
sudo systemctl enable ssh
```

If you've never used or heard of it before, [systemctl](https://www.man7.org/linux/man-pages/man1/systemctl.1.html) is a program which controls something called [systemd](https://en.wikipedia.org/wiki/Systemd).

You can think of them as basically the fundamental controllers of everything going on in the operating system.  If you're familiar with Windows, it's not an exactly 1:1 comparison but you can think of them as similar to task manager and the registry.   

You'll want to make sure that you allow all SSH connections through your firewall.  There are a lot of very specific ways you can allow this through exact ports and protocols, but the simplest (and the only one we'll discuss right now) is to use the pre-established application profile for SSH.  It'll take care of all that stuff for you automatically.

```bash
sudo ufw allow ssh
```

Now run this command:

```bash
sudo ufw status
```

In the output you should see that it is `active` and that SSH is in a list with the action `ALLOW` from `Anywhere`.  

That's it for configuring your server machine as an SSH server.

Now let's make sure you've got everything you need to connect to it from a client.

Before moving on make sure you know your username and password for this machine.  you can check your username with `whoami`

```bash
whoami
```

It will echo your username back to you.

### get your IP address

TODO: there's another tutorial for this

### connect to SSH from a client

Switch over to the machine you'll be using to connect.  If it's Windows, use git bash or an equivalent terminal.

Git bash will come preinstalled with the SSH client.  Mac should have it installed as well.  

If the machine you're using to connect to your server is also Linux, but doesn't have it installed, you can install it with:

```bash
sudo apt install openssh-client
```

Then you can connect to the server with your `{USERNAME}@{IP_ADDRESS}` like so:

```bash
ssh eagleson@10.0.0.48
```

The first time you run this it will ask if you recognize this fingerprint


```bash
eagleson@eagleson-OptiPlex-7040:~$ ssh eagleson@10.0.0.48

The authenticity of host '10.0.0.48 (10.0.0.48)' can't be established.
ED25519 key fingerprint is SHA256:TiY98ZoH5ORRoaKZaa78d1ZcVYiuKu1NBG4NI+NYS80.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

You'll want to say yes.  But what does this mean?

## COOL ASIDE

If you were to go down to your machine you can use `ssh-keygen` to see the fingerprint

```bash
eagleson@eagleson-OptiPlex-7040:~$ ssh-keygen -l -f /etc/ssh/ssh_host_ed25519_key.pub

256 SHA256:TiY98ZoH5ORRoaKZaa78d1ZcVYiuKu1NBG4NI+NYS80 root@eagleson-OptiPlex-7040 (ED25519)
```

You'll see this part `TiY98ZoH5ORRoaKZaa78d1ZcVYiuKu1NBG4NI` matches what we were shown when we tried to connect to the server, which means we can feel confident that the server we are connecting to is the one we want and not someone else who has an IP of something we may have mistyped

https://ociaw.com/posts/determining-ssh-host-key-fingerprint#:~:text=ssh-keygen%20-l%20-f%20%2Fetc%2Fssh%2Fssh_host_ecdsa_key.pub%20You%20can%20then%20compare,determine%20what%20the%20issue%20is%20and%20resolve%20it.

Once thats done once you wont have to agree again unless the fingerprint changes.  That host will be added to a file you can track stored in `~/.ssh/known_hosts`

You can print the contents of that file to the terminal with:

```bash
cat ~/.ssh/known_hosts
```

### END COOL ASIDE

Once you have said yes and agree you are now logged in as that user on your server and can basically do anything you want (that the user can do).

Congratulations!  

At this stage you _could_ go unplug the mouse, monitor and keyboard from your server.  You technically won't need them anymore.

## Setup your own SSH keys so you dont need passwords TODO later

That said, it's absolutely not necessary to use at all, just a nice convenience thing. The below links show you how to generate your own keys on the command line completely free:

[How to generate SSH keys on Windows](https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html#CreatingSSHkeys-CreatinganSSHkeyonWindows)
[How to generate SSH keys on macOS/Linux](https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html#CreatingSSHkeys-CreatinganSSHkeyonLinux&macOS)

