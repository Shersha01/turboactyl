![turboactyl](https://cdn.discordapp.com/attachments/930903758777499678/946010248869724210/360_F_276639635_TmXufACSbROI26srcqfppqZuCYzmI0ob-removebg-preview.png)

<hr>

# **Disclaimer**
We are not responsible for any damages.

# Turboactyl V1 • The best Dashactyl fork
Making a free or paid host and need a way for users to sign up, earn coins, manage servers? Try out turboactyl.
To get started, scroll down and follow the guide

All features:
- Resource Management (gift, use it to create servers, etc)
- Coins (AFK Page earning, Join for Rewards)
- Coupons (Gives resources & coins to a user)
- Servers (create, view, edit servers)
- User System (auth, regen password, etc)
- Store (buy resources with coins)
- Dashboard (view resources & servers from one area)
- Join for Resources (join discord servers for resources)
- Admin (set/add/remove coins & resources, create/revoke coupons)
- API (for bots & other things)
- Legal (tos/pp in footer & its own page)
- Webhook (Logs actions)
- Gift Resources (Share resources with anyone)

# Warning

We cannot force you to keep the "Powered by Turboactyl" in the footer, but please consider keeping it. It helps getting more visibility to the project and so getting better. We won't do technical support for installations without the notice in the footer.


# Install Guide (pt. 1)

Warning: You need Pterodactyl already set up on a domain for turboactyl to work
1. Run `sudo apt update && sudo apt install git`
2. Run `curl -fsSL https://deb.nodesource.com/setup_12.x | sudo bash - && apt install nodejs`
2. Run `git clone https://github.com/turboactyl/turboactyl.git && cd turboactyl && npm install`
3. Configure settings.json (specifically panel domain/apikey and discord auth settings for it to work)
4. Start the server (Ignore the 2 strange errors that might come up)

# Install Guide (pt. 2)

1. Login to your DNS manager, point the domain you want your dashboard to be hosted on to your VPS IP address. (Example: dashboard.domain.com 192.168.0.1)
2. Run `apt install nginx && apt install certbot` on the vps
3. Run `ufw allow 80 && ufw allow 443` on the vps
4. Run `certbot certonly -d <Your turboactyl Domain>` then do 1 and put your email
5. Run `nano /etc/nginx/sites-enabled/turboactyl.conf`
6. Paste the configuration at the bottom of this and replace with the IP of the pterodactyl server including the port and with the domain you want your dashboard to be hosted on.
7. Run `systemctl restart nginx` and try open your domain.
# Nginx Proxy Config
```Nginx
server {
    listen 80;
    server_name <domain>;
    return 301 https://$server_name$request_uri;
}
server {
    listen 443 ssl http2;
location /afkwspath {
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
  proxy_pass "http://localhost:<port>/afkwspath";
}
    
    server_name <domain>;
ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
location / {
      proxy_pass http://localhost:<port>/;
      proxy_buffering off;
      proxy_set_header X-Real-IP $remote_addr;
  }
}
```
<hr>

