# Error Pages ( English )

In this video, we will explore how to customize NGINX behavior when things go wrong, specifically handling server errors like a 500 error and customizing the user display for a 404 error. When a user encounters a 404 error, it means the content they are looking for isn't available. Currently, NGINX has a default 404 page, but we can and should customize this page for our web service or website to maintain consistent branding.

First, we will check what happens when a 404 error occurs by using the `curl localhost` command for a non-existent file, such as `fake.html`. NGINX provides a basic 404 page by default, but we can override this with our custom page. Viewing this page in a web browser might make it recognizable from other services, but we want a custom version tailored to our site.

Next, we’ll modify the `conf.d/default.conf` file in the server block to set up custom error pages. By looking at the NGINX documentation, we find the `error_page` directive. This directive allows us to specify a list of HTTP status codes and the corresponding URI to render a custom page. For instance, to customize the 404 page, we add the line `error_page 404 /404.html` to our server block. We then need to create the `/usr/share/nginx/html/404.html` file with a simple HTML structure, such as `<h1>404 Page Not Found</h1>`.

After making these changes, we reload NGINX using `systemctl reload nginx`. Now, when we curl `localhost/fake.html`, we will see our custom 404 page.

Finally, we set up custom error pages for server errors like 500 through 504. By adding the line `error_page 500 501 502 503 504 /50x.html` to the server block, we ensure that any of these errors will render the `/50x.html` page. Since the `50x.html` page already exists, we save our changes and reload NGINX to apply them. Although we don’t simulate a 500 error here, we trust that the setup works as it did for the 404 page.

By following these steps, we can provide a better user experience by displaying custom error pages that align with our site's branding.