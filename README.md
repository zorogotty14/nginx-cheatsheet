# NGINX Cheatsheet (Reverse Proxy & Load Balancing)

## **NGINX Overview**
NGINX is a high-performance web server, reverse proxy, and load balancer commonly used for serving websites and managing traffic.

---

## **NGINX Installation**
### **For Debian/Ubuntu**
```sh
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### **For CentOS/RHEL**
```sh
sudo yum install epel-release -y
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### **Verify Installation**
```sh
nginx -v                  # Check NGINX version
systemctl status nginx    # Check NGINX service status
```

---

## **Basic NGINX Commands**
```sh
sudo systemctl start nginx      # Start NGINX
sudo systemctl stop nginx       # Stop NGINX
sudo systemctl restart nginx    # Restart NGINX
sudo systemctl reload nginx     # Reload configuration without downtime
```

---

## **NGINX Configuration Basics**
NGINX configuration is stored in `/etc/nginx/nginx.conf` and additional files in `/etc/nginx/sites-available/`.

### **Basic NGINX Server Block (Virtual Host)**
```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
```sh
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
sudo systemctl reload nginx
```

---

## **Setting Up NGINX as a Reverse Proxy**
```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
```sh
sudo systemctl reload nginx
```

---

## **Setting Up NGINX Load Balancer**
```nginx
upstream backend {
    server backend1.example.com;
    server backend2.example.com;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
```
```sh
sudo systemctl reload nginx
```

---

## **Enable HTTPS with Let’s Encrypt**
```sh
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d example.com -d www.example.com
sudo systemctl reload nginx
```

---

## **NGINX Logging**
```sh
sudo tail -f /var/log/nginx/access.log  # View access logs
sudo tail -f /var/log/nginx/error.log   # View error logs
```

---

## **Useful NGINX Resources**
- [NGINX Docs](https://nginx.org/en/docs/)
- [NGINX Config Generator](https://www.digitalocean.com/community/tools/nginx)
- [NGINX Best Practices](https://www.nginx.com/resources/library/)

---

This cheatsheet provides a quick reference for NGINX setup, reverse proxy, load balancing, and HTTPS configuration. Let me know if you need any modifications!

