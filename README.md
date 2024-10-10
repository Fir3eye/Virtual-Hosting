# Virtual-Hosting
How to Host Multiple Websites on One Server with NGINX | Step-by-Step Guide
![image](https://github.com/user-attachments/assets/80e8da4e-6137-4eee-8d83-35a16332f55a)

## Step 1: Create Separate Directories for Each Website
- Organize your websites by creating separate directories for each one. For example, let’s say you want to host two websites: example1.com and example2.com.
```
sudo mkdir -p /var/www/example1.com/html
sudo mkdir -p /var/www/example2.com/html

```
- Set the appropriate permissions:

## Step 2: Create Sample HTML Pages for Each Website
- Create a simple index.html for each website to verify they’re being served correctly.

- Insert the following content:`For example1.com:`

```
nano /var/www/example1.com/html/index.html
```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Welcome to Example1.com</title>
</head>
<body>
    <h1>Success! You are viewing Example1.com</h1>
</body>
</html>

```
- Insert the following content:`For example1.com:`
```
nano /var/www/example2.com/html/index.html

```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Welcome to Example2.com</title>
</head>
<body>
    <h1>Success! You are viewing Example2.com</h1>
</body>
</html>

```
## Step 3: Install NGINX (If Not Already Installed)

- For Ubuntu/Debian:
```
sudo apt update
sudo apt install nginx
```

- Start and enable NGINX to run on boot:

```
sudo systemctl start nginx
sudo systemctl enable nginx
```

## Step 4: Configure NGINX Server Blocks for Each Website
- For  `example1.com`
```
sudo nano /etc/nginx/sites-available/example1.com
```
- Insert the following configuration:

```
server {
    listen 80;
    listen [::]:80;

    server_name example1.com www.example1.com;

    root /var/www/example1.com/html;
    index index.html;

    access_log /var/log/nginx/example1.com.access.log;
    error_log /var/log/nginx/example1.com.error.log;

    location / {
        try_files $uri $uri/ =404;
    }
}

```
- For  `example2.com`

```
sudo nano /etc/nginx/sites-available/example2.com
```
- Insert the following configuration:

```
server {
    listen 80;
    listen [::]:80;

    server_name example2.com www.example2.com;

    root /var/www/example2.com/html;
    index index.html;

    access_log /var/log/nginx/example2.com.access.log;
    error_log /var/log/nginx/example2.com.error.log;

    location / {
        try_files $uri $uri/ =404;
    }
}

```

## Step 5: Enable the Server Blocks
- Create symbolic links from sites-available to sites-enabled to enable the server blocks.
```
sudo ln -s /etc/nginx/sites-available/example1.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/example2.com /etc/nginx/sites-enabled/

```

## Step 6: Test NGINX Configuration

```
sudo nginx -t

```

## Step 7: Reload NGINX to Apply Changes

```
sudo systemctl reload nginx

```
## Step 8: Update DNS Settings for Each Domain

## Step 9: (Optional) Secure Your Websites with SSL/TLS
- Install Certbot:
```
sudo apt install certbot python3-certbot-nginx

```
- For `example1.com`
```
sudo certbot --nginx -d example1.com -d www.example1.com

```

- For `example2.com`
```
sudo certbot --nginx -d example2.com -d www.example2.com

```
## Step 10: Access Your Websites
- Once DNS propagation is complete:

    - Visit http://example1.com or https://example1.com to see the first website.
    - Visit http://example2.com or https://example2.com to see the second website.
