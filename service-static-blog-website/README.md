## server

sudo apt install nginx


sudo ufw allow 'Nginx Full'


 sudo nano /etc/nginx/sites-enabled/default

```
server {
        listen 80 default_server;
        return 444;
}

server {
        listen 80;
        server_name www.eagleson.duckdns.org eagleson.duckdns.org;

        location /jellyfin/ {
                proxy_pass http://127.0.0.1:8096/;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
        }
}
```




```
sudo apt install openssl
```

https://linuxconfig.org/how-to-generate-a-self-signed-ssl-certificate-on-linux



```
openssl req -newkey rsa:4096  -x509  -sha512  -days 365 -nodes -out certificate.pem -keyout privatekey.pem
```



https://medium.com/@m.fareed607/how-to-set-up-an-nginx-reverse-proxy-server-and-enable-https-with-certbot-bbab9feb6338




https://www.linode.com/docs/guides/enabling-https-using-certbot-with-nginx-on-ubuntu/


```
sudo apt install certbot python3-certbot-nginx
```

```
sudo certbot --nginx -d eagleson.duckdns.org
```


these get added:

        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/eagleson.duckdns.org/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/eagleson.duckdns.org/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


## site

download from https://github.com/gohugoio/hugo/releases

im grabbing the AMD64 version


echo $PATH and see what there is

I see 

/c/Users/alexe/bin

I had to make the `/bin` folder in users and put `hugo.exe` in there from the zip


```
hugo new site quickstart
cd quickstart
```

downlaoding theme:

```
cd themes
mkdir PaperMod
cd PaperMod
git clone https://github.com/adityatelange/hugo-PaperMod themes/PaperMod --depth=1
```

to update just navigate to that directory and `git pull`

```
theme = "PaperMod"
```

run `hugo server` from the directory


now content

```
hugo new content content/posts/my-first-post.md
```

run the following to run the server with draft content

```
hugo server -D
```



## mvoing to linux for hosting

install hugo

```
sudo apt update
sudo apt install hugo
```

hosting with nginx

move folder where you want it and make sure to chmod 755 the folder (was defualt for me)

create a new file in `sites-available`

```bash
sudo nano /etc/nginx/sites-available/family-website
```

and add this:


```conf
server {
    listen 80;
#   server_name my-epic-website.com www.my-epic-website.com;

    root /var/www/family-website/public;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```


then sym,link it .  make sure NOT to use relative links

```bash
sudo ln -s /etc/nginx/sites-available/family-website /etc/nginx/sites-enabled/
```

