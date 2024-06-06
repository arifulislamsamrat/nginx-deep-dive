# Installing NGINX on Ubuntu 16.04 English

we will go through the steps to install the latest stable version of NGINX on an Ubuntu 16.04 machine. If you have already installed NGINX on a CentOS machine and plan to use that, you can mark this video as complete and continue with the course. If you prefer using Ubuntu, follow along with the steps below.

### Setting Up the Server

1. **Create the Server**:
    - Click on Cloud Servers.
    - Select Ubuntu 16 from the dropdown.
    - Click Create the Server.
2. **Login to the Server**:
    - Once the server has started, click Login.
    - In the terminal, log in as the default user with the username "user" and password "123456".
    - Re-enter the password to set a new one.

### Updating and Installing NGINX

1. **Update Packages**:
    - Update the packages to get everything up to date.
2. **Check Existing NGINX**:
    - Use `apt-cache` to check if NGINX is available, but note that the default version might be outdated.
3. **Add Official NGINX Repository**:
    - Go to [NGINX.org](http://nginx.org/) for official documentation.
    - Note the codename for Ubuntu 16.04 is "xenial".
    - Get the signing key and add it to the apt key registry:
        
        ```bash
        wget -qO - <https://nginx.org/keys/nginx_signing.key> | sudo apt-key add -
        ```
        
    - Add the repository to the apt sources list:
        
        ```bash
        echo "deb <http://nginx.org/packages/ubuntu/> xenial nginx" | sudo tee /etc/apt/sources.list.d/nginx.list
        echo "deb-src <http://nginx.org/packages/ubuntu/> xenial nginx" | sudo tee -a /etc/apt/sources.list.d/nginx.list
        ```
        
4. **Install NGINX**:
    - Update the apt package list:
        
        ```bash
        sudo apt-get update
        ```
        
    - Install NGINX:
        
        ```
        sudo apt-get install nginx
        
        ```
        

### Starting and Enabling NGINX

1. **Start NGINX**:
    - Use systemd to start NGINX:
        
        ```bash
        sudo systemctl start nginx
        ```
        
    - Enable NGINX to start on boot:
        
        ```bash
        sudo systemctl enable nginx
        ```
        
2. **Verify NGINX Installation**:
    - Stop and restart the server from the Linux Academy Cloud Server listing.
    - Once restarted, copy the public IP address of the server.
    - Open the IP address in a browser to verify the NGINX HTML page is served.

Now, you have NGINX running on Ubuntu 16.04 and are ready to continue with the rest of the course.