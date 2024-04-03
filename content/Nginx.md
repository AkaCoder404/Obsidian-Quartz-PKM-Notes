



```shell
sudo systemctl reload nginx
sudo systemctl restart nginx

sudo tail -f /var/log/nginx/error.log

```

```
sudo ln -s /etc/nginx/sites-available/your_config_file /etc/nginx/sites-enabled/

sudo tail /var/log/nginx/access.log

```

```
server {
    listen 80;
    server_name your_domain_or_IP;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```


**On AWS Instance**
Have to open EC2 instances' firewall ports (iptables) as well as the security groups.
```
sudo firewall-cmd --list-all
sudo firewall-cmd --permanent --add-port=3000/tcp
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```
