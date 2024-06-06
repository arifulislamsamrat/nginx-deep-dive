# Use Case: Generating SSL Certificates using Let's Encrypt English

Here we have discussed  a practical use case for setting up a website with free, automated SSL certificates using Let's Encrypt and NGINX. This tutorial isn't strictly a follow-along guide because it requires owning a domain to configure DNS records. Despite this, the video aims to demonstrate the entire process, showcasing the capabilities of Let's Encrypt.

### Let's Encrypt Overview

Let's Encrypt is a certificate authority that provides free SSL certificates, making it accessible for anyone to secure their websites. The service offers an API for automating the creation and renewal of these certificates. Unlike traditional SSL certificates that can last for a year or more, Let's Encrypt certificates are valid for 90 days, requiring regular renewals. However, these free certificates are of high quality and can achieve an A-plus SSL rating, similar to paid certificates.

### Tools Used

The primary tool demonstrated in the video is Certbot, developed by the Electronic Frontier Foundation (EFF). Certbot simplifies the process of obtaining and renewing SSL certificates from Let's Encrypt. It includes a plugin that integrates directly with NGINX, automating the configuration needed to secure your website.

### Initial Setup

To begin, you need to set up the necessary DNS records using a DNS management service such as dnsimple. This involves creating an A record for the root domain and a CNAME record for the www subdomain, with a short time-to-live (TTL) to speed up DNS propagation. Following this, Certbot and its NGINX plugin are installed using package managers like `yum install -y certbot-nginx`.

### Configuring NGINX

The next step involves creating a basic NGINX configuration file. This configuration, saved in `conf.d/domain.conf`, sets up a server block to listen on port 80 and serve the default NGINX page. The video uses the example domain `chord.tools` for this demonstration. After configuring NGINX, Certbot is run with the `--nginx` flag and the specified domains. During this process, Certbot prompts for an email address for notifications, requires agreement to the terms of service, and performs a domain validation challenge by modifying the NGINX configuration to serve a specific validation file.

### Post-Configuration

Upon successful domain validation, Certbot updates the NGINX configuration with the necessary SSL settings. This includes paths to the SSL certificate and key files, as well as configuration directives to handle HTTPS traffic and redirects. The updated configuration file is more detailed but essentially achieves the same outcome as the initial simple configuration.

To ensure the certificates are renewed automatically, Certbot's integration with systemd is utilized. The EPEL release repository provides service and timer units for certbot-renew. The timer is set to check for certificate renewal daily. Additionally, a post-renewal hook is configured to reload NGINX after the certificates are renewed. This is done by editing the `/etc/sysconfig/certbot` file to include the post-hook command `systemctl reload nginx`.

In conclusion, the video demonstrates the ease and effectiveness of using Let's Encrypt and Certbot to secure a website with automated SSL certificates. This method not only provides a cost-free solution but also enhances web security by making HTTPS more accessible. Since its introduction, Let's Encrypt has significantly increased the amount of HTTPS traffic on the web, and it's a highly recommended tool for anyone looking to secure their website without the expense of paid certificates.