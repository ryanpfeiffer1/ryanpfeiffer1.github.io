---
layout: post
title: "How to put a website on the web"
date: 2025-04-05 9:00:00
---

# **How to Put a Website Online for Free**

## **Introduction**

So you\'ve created a website, but now what? How do you actually get it
online for everyone to see? This guide will walk you through setting up
a free Virtual Private Server (VPS), registering a free domain, and
hosting a Node.js website---all without spending a dime.

### **What This Guide Covers:**

- Configuring a free VPS from Oracle Cloud

- Registering a free domain from YDNS.io

- Hosting a free Node.js website

### **Understanding the Basics**

Before diving in, let\'s clarify some key concepts:

**1. Domain Name**

A domain name is the text users type to reach your website. Examples
include:

- google.com

- amazon.com

- facebook.com

**Free Options:**

**2. Web Server**

A web server is a computer that hosts your website, making it accessible
to users online. Popular providers include:

- Linode

- Google Cloud

- Microsoft Azure

**Free Option:**

- Oracle Cloud Free Tier

## **Step-by-Step Guide to Hosting Your Website**

**Step 1: Create a Free VPS on Oracle Cloud**

1.  Navigate to and sign up.

2.  Complete the registration (a small one-time fee may apply).

3.  In the Oracle Cloud dashboard, navigate to **Instances**.

4.  Click **Create Instance** and name it \"MyWebsite\".

5.  Under **Instance and Shape**, select:

    - Image: *Canonical Ubuntu 20.04*

    - Shape: *VM.Standard.E2.1.Micro*

<!-- -->

1.  Under **Add SSH Keys**, save the private key in a memorable
    location.

2.  Click **Create**.

**Note:** Oracle might display a cost warning, but as long as you are on
the \"Always Free\" tier, you won't be charged.

**Step 2: Connect to Your VPS**

1.  Navigate to your **MyWebsite** instance page in Oracle Cloud.

Open a terminal and run the following commands:  
```bash
chmod 600 \[your-key\].key

ssh -i \[your-key\].key ubuntu@\[your-server-ip]
```
**Step 3: Set Up Your Node.js Server**

Update system packages:
```bash
sudo apt update && sudo apt upgrade -y
```
Install Node.js:  
```bash
curl -fsSL https://deb.nodesource.com/setup_16.x \| sudo -E bash -
sudo apt install -y nodejs
```
Install Nano (a text editor):  
```bash
sudo apt install nano
```
Create a basic test server:  
```bash
mkdir server && cd server

touch server.js

nano server.js
```
Copy and paste the following code into server.js:  
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, world!');
});

server.listen(3000, '127.0.0.1', () => {
  console.log('Listening on 127.0.0.1:3000');
});

```
1.  Save and exit (Ctrl+X, then Y, then Enter).

Start the server:  
```bash
node server.js &
```
**Step 4: Register a Free Domain on YDNS.io**

1.  Navigate to and create an account.

2.  Create a new domain with any name you choose.

3.  In the \"Content\" field, enter your Oracle Server's public IP
    address.

**Step 5: Configure Firewall Rules on Oracle Cloud**

1.  On your **MyWebsite** instance page, click the link next to
    **Virtual Cloud Network**.

2.  Scroll down and click the link under **Subnets**.

3.  Click the link under **Security Lists**.

4.  Add two ingress rules:  
      
    **Rule 1:**

    - **Source CIDR:** 0.0.0.0/0

    - **Destination Port:** 80

    - **Description (Optional):** Allow HTTP Access

    **Rule 2:**

    - **Source CIDR:** 0.0.0.0/0

    - **Destination Port:** 443

    - **Description (Optional):** Allow HTTPS Access


5. Apply changes and return to your server.

6. Run:
    ```bash
    sudo -i

    iptables -F

    netfilter-persistent save
    ```
**Step 6: Set Up Nginx as a Reverse Proxy**

Install Nginx:  
  
```bash
sudo apt install nginx

sudo systemctl enable nginx

sudo systemctl start nginx
```
Create a new Nginx configuration file:  
```bash
sudo nano /etc/nginx/sites-enabled/proxy.conf
```
Paste the following configuration:  
```bash
server {

listen 80;

server_name yourdomain.com;

location / {

proxy_pass http://127.0.0.1:3000;

proxy_set_header Host \$host;

proxy_set_header X-Real-IP \$remote_addr;

proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;

}

}
```
1.  Replace yourdomain.com with your actual domain from YDNS.io.

2.  Save and exit.

Restart Nginx:  
```bash
sudo systemctl restart nginx
```
Restart your Node.js server:  
```bash
nohup node server.js &
```
1.  Visit your domain name in a browser. If you see \"Connection
    Reset,\" proceed to the next step.

**Final Step: Secure Your Website with HTTPS**

Install Certbot:  
```bash
sudo snap install --classic certbot

sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
Run Certbot to obtain an SSL certificate:  
```bash
sudo certbot --nginx
```
- Press **Enter** to skip email.

- Press **Y** to accept the terms.

- Press **1** to enable HTTPS.

Test auto-renewal:  
```bash
sudo certbot renew --dry-run
```
Now visit your domain, and you should see your website online.

**What's Next?**

Now that your website is live, you can:

- Replace the test server code with your own Node.js website.

- Use AI tools or templates to generate a more dynamic site.

- Consider upgrading to paid hosting if you need more power.

This free setup is perfect for budget-conscious developers looking for a fast, reliable hosting solution. 

Enjoy your new website!