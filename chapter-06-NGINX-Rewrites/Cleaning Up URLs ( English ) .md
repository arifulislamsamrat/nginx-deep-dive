# Cleaning Up URLs ( English )

In this lesson, we explore how to use rewrite rules with NGINX. Rewrite rules let us modify incoming URLs, either by changing and redirecting them to a desired format or by altering them before passing them to location blocks for handling.

We will remove the **`.html`** file extension from URLs while ensuring the same content is served. This is often done for vanity URLs, which look cleaner and hide underlying infrastructure details.

### **Steps:**

1. **Understanding Rewrite Rules:**
    - Rewrite rules are used within server or location blocks.
    - They rely on regular expressions to define patterns and capture groups.
    - The captured groups can be referenced in the replacement string using **`$1`**, **`$2`**, etc.
2. **Regular Expressions Basics:**
    - We use **`^`** to denote the start of the string.
    - Parentheses **`()`** create capture groups.
    - **`.*`** matches zero or more characters.
    - **`\.`** escapes the dot character to match a literal period.
    - **`?`** makes the preceding element optional.
    - **`$`** denotes the end of the string.
3. **Creating Rewrite Rules:**
    - We ad rewrite rules in the **`conf.d/default.conf`** file, right after our SSL certificates.
    - The first rewrite rule strips **`.html`** from URLs and redirects using a 302 status code:
        
        ```bash
        rewrite ^/(.*)\.html(\?.*)?$ /$1$2 redirect
        ```
        
    - The second rewrite rule handles URLs ending with a trailing slash:
        
        ```bash
        rewrite ^/(.*)/$ /$1 redirect;
        ```
        
4. **Testing the Configuration:**
    - Save the changes and test the configuration with **`nginx -t`**.
    - Reload NGINX with **`systemctl reload nginx`**.
5. **Fixing Potential Redirect Loops:**
    - If encountering redirect loops, ensure there's a location block to handle the resulting URLs.
6. **Using `try_files` Directive:**
    - This directive checks for the existence of specific files and serves content accordingly without altering the URL.
    - Add a location block to handle requests after rewrites:
        
        ```bash
        location / {
          try_files $uri $uri/ $uri.html =404;
        }
        ```
        

### **Example Configuration:**

1. Open **`conf.d/default.conf`** and add the rewrite rules:
    
    ```bash
    rewrite ^/(.*)\.html(\?.*)?$ /$1$2 redirect;
    rewrite ^/(.*)/$ /$1 redirect;
    ```
    
2. Add the **`try_files`** directive inside the location block:
    
    ```bash
    location / {
      try_files $uri $uri/ $uri.html =404;
    }
    ```
    
3. Test and reload NGINX:
    
    ```bash
    nginx -t
    systemctl reload nginx
    ```
    
4. Verify by accessing your server's URLs. URLs with **`.html`** should now be stripped of the extension and still serve the correct content.

By following these steps, you can successfully implement URL rewriting in NGINX to create cleaner, more user-friendly URLs while maintaining the same functionality.