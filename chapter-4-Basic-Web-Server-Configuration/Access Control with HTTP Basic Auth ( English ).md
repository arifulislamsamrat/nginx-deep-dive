# Access Control with HTTP Basic Auth ( English )

In this video, we are going to take a look at how we can take one of the virtual servers that we're working on and specify different actions based on the given path. Specifically, we will set up a path that requires authentication using HTTP basic auth, ensuring you know how to secure areas with this type of authentication.

To get started, we'll learn how to handle different paths. This involves using the **`location`** directive in the core module, rather than the **`error_page`** directive we've previously discussed. The **`location`** directive allows us to create a specific block of configuration that activates when there is a matching URI. This match can be exact, using an **`=`** sign, or it can involve various regular expression-based matches.

For this example, we want users accessing **`/admin.html`** to log in before viewing the content. We'll first set up our **`location`** block and then handle the authentication. We'll go back into **`conf.d`** and edit **`default.conf`**. I prefer keeping error pages at the bottom, so we'll add the **`location`** block above them.

We'll use **`=`** to specify an exact match. If it matches exactly **`/admin.html`**, it will fall into this specific block. The basic auth configuration is in a different module, so we'll refer to the **`auth_basic_module`** documentation. This module has only two directives: **`auth_basic`** and **`auth_basic_user_file`**.

The **`auth_basic`** directive takes a string, which is the message presented to users when they are prompted to log in. It could be something like "Login Required" or "Password Required for Access". However, if the value is **`off`**, it disables basic authentication. This is disabled by default. The **`auth_basic_user_file`** directive points to a file containing usernames and their associated encrypted passwords.

To create this file, we’ll use the **`htpasswd`** utility, which can be installed on most systems. While you can use **`openssl`** to generate a password, **`htpasswd`** is preferable. We need to create the file and set up both directives in our configuration.

First, in the configuration file, we add:

```bash
location = /admin.html {
    auth_basic "Login Required";
    auth_basic_user_file /etc/nginx/.htpasswd;
}
```

Next, we generate the password file. We'll install **`httpd-tools`** on a CentOS/RHEL system or **`apache2-utils`** on Ubuntu/Debian. Once installed, we use **`htpasswd`** with the **`-c`** flag to create the file:

```bash
htpasswd -c /etc/nginx/.htpasswd admin
```

We'll be prompted to enter a password and confirm it. After creating the file, we'll check our configuration for errors and reload NGINX with:

```bash
systemctl reload nginx
```

To test, we'll use **`curl`** to access **`localhost/admin.html`**. This will return a 401 authorization required because **`curl`** doesn't handle authentication prompts by default. We'll need to pass the user credentials with:

```bash
curl -u admin:password localhost/admin.html
```

If we get a 404 Not Found, it means the **`admin.html`** file doesn’t exist yet. We'll create it with simple content like "Admin Page" and place it in **`/usr/share/nginx/html/admin.html`**. Trying the **`curl`** command again should display the admin page.

When accessing from a browser, it will prompt for a username and password, displaying our "Login Required" message. Entering the correct credentials will show the admin page.

Let's review the configuration in **`default.conf`**. Inside the **`location`** block, we specified that requests to **`/admin.html`** require authentication using **`auth_basic`** and **`auth_basic_user_file`**. If accessing other paths, they will be served as usual without authentication prompts.