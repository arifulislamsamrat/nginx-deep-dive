# Redirecting All trafic to HTTPS ( English )

### Notes on Redirecting HTTP Traffic to HTTPS with NGINX

### Overview

In this lesson, we learn how to redirect all HTTP traffic to HTTPS using NGINX. This is an important practice for securing web traffic and optimizing website performance.

### Key Points

1. **Importance of HTTPS:**
    - HTTPS provides security and is now a standard practice.
    - Using HTTPS was once a performance hit, but modern improvements, especially with HTTP/2, have mitigated this issue.
    - HTTPS offers security benefits and boosts search engine rankings.
2. **Using Rewrite Rules and Return Keyword:**
    - The `rewrite_module` in NGINX allows us to use the `return` keyword to handle redirects.
    - The `return` keyword can return a status code, a code with a URL, or just a URL.
3. **Step-by-Step Implementation:**
    - **Separate Server Blocks:**
        - Create a separate server block listening on port 80 (HTTP).
        - Configure this block to return a 404 error for any HTTP requests initially.
    - **Testing the Configuration:**
        - Save changes and test with `nginx -t`.
        - Reload NGINX with `systemctl reload nginx`.
        - Verify by accessing HTTP URLs, which should return a 404 error.
    - **Setting Up Redirection:**
        - Change the 404 response to a 301 redirect, sending traffic to HTTPS.
        - Use NGINX variables (`$host` and `$request_uri`) to ensure the URL structure remains intact.
        - Example configuration:
            
            ```bash
            server {
                listen 80;
                server_name yourdomain.com;
                return 301 https://$host$request_uri;
            }
            ```
            
    - **Testing the Redirection:**
        - Save and quit the configuration file.
        - Test and reload NGINX.
        - Verify by accessing HTTP URLs, which should now redirect to HTTPS.
4. **Benefits of HTTP to HTTPS Redirection:**
    - **Security:** All data is encrypted, protecting user information.
    - **Performance:** HTTP/2, which requires HTTPS, improves load times.
    - **SEO:** Search engines prioritize secure sites, improving visibility.

By following these steps, you can effectively redirect HTTP traffic to HTTPS, enhancing both the security and performance of your website. Understanding how to utilize NGINX's `return` function and variables is crucial for managing secure web traffic.