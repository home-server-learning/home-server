use this link

https://docs.docker.com/engine/install/ubuntu/

```bash
cd ~
nano docker-repo.sh
```


check this link first for updates code:

https://docs.docker.com/engine/install/ubuntu/#installation-methods


you should use the most up to date script from there prefer to the below

then paste it:

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

so you see a few thigns going on there

learn sources:

https://en.wikipedia.org/wiki/APT_(software)#Sources

curl is a tool for getting/sending data via a URL.  similar to like a command line version of stuff you would do by clicking links and buttons o na webpage, including "download this" buttons

https://en.wikipedia.org/wiki/CURL#curl

GPG keys help make sure the server you are talking to is legit

https://en.wikipedia.org/wiki/GNU_Privacy_Guard#Overview

so all the above will just make sure your machine has the tools it needs to download and install the most up to date version of the official docker

you will need to do

```bash
chmod +x docker-repo.sh
```

just so you can run it.  then ru nit:

```bash
docker-repo.sh
```

and the below will actually install it

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

again, check the link above for the most recent versions

once done you should be able to do:

```bash
docker -v
```

im not gonna paste it but you should get tons of stuff instead of "command docker not found"

now lets test running a container:

check the docs here:

https://hub.docker.com/_/hello-world/

```bash
$ sudo docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete
Digest: sha256:53cc4d415d839c98be39331c948609b659ed725170ad2ca8eb36951288f81b75
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

...
```

there's more it'll spit out but "Hello from Docker!" is really what you are looking for

this tells you that you have now properly configured your system to run containers and basically opened up a massive world of tools to run without having to install stuff on your machine.

if you're following along with this project we might want to take this time to make sure we can sync on both client and server machines a git repository

---

if something goes wrong

docker logs --since=1h 'container_id'

## UFW

UFW docker tutorial

https://github.com/chaifeng/ufw-docker