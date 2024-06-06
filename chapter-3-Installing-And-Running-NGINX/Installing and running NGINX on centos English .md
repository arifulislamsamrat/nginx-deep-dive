# Installing and running NGINX on centos English

we are going to start working with NGINX by setting up a server running NGINX. We will use the Linux Academy cloud servers and set up NGINX on a CentOS 7 machine from scratch. For those interested in using a Debian-based system like Debian or Ubuntu, there will be a separate video covering the same process. However, for the rest of the course, we will use a CentOS server. If you are using a different system, the differences should be minor and unrelated to NGINX.

### Setting Up the Server

First, click on Cloud Servers and create a server with CentOS 7 as the default. Once the server is created, you will have public and private IPs, and we will use the web terminal to interact with the server. Click the login button to open a web terminal. If you see a "not running" page, wait for the DNS to propagate and refresh.

### Logging In

Sign in using "user" as the login and "123456" as the password. Re-enter the password, then set a new password for the remainder of the course. Now, you are signed in as a standard user with sudo privileges. For most of the course, we will switch to the root user to avoid using sudo for nearly every command.

### Configuring NGINX Repository

First, set up a new yum repository to install the official compiled NGINX version from [NGINX.org](http://nginx.org/). This documentation will be crucial throughout the course. To do this, visit [NGINX.org](http://nginx.org/) and navigate to /en/linux_packages.html. Copy the relevant instructions for the stable version and paste them into /etc/yum.repos.d/nginx.repo using vim. Replace the placeholder values with "CentOS" and "7" for OS and OS release, respectively. Save and quit using :qw.

### Updating and Installing NGINX

Run `sudo yum update` to load the new repository. After the update, install NGINX using `sudo yum install -y nginx`. The -y flag automatically confirms the installation.

### Starting NGINX

Once NGINX is installed, use systemd to run the NGINX server and start it on system boot. Execute `sudo systemctl start nginx` and `sudo systemctl enable nginx`. To verify, curl localhost to see the NGINX welcome page. Copy the public IP address of your server and paste it into a browser to see the HTML render of the page.

Now, we have NGINX installed and running by default on one of the Linux Academy cloud servers. If the server stops and restarts, NGINX will run by default.