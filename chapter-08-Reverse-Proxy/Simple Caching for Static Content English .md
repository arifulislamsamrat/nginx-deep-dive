# Simple Caching for Static Content English

We've discussed setting up reverse proxy servers with NGINX, focusing on how to manage a reverse proxy for caching to improve performance by reducing the load on application servers. By caching known responses, NGINX can serve content directly without needing to request it from the application server repeatedly, which is particularly beneficial under heavy load. NGINX, being highly performant and optimized, handles multiple requests simultaneously much better than application servers running languages like Ruby on Rails, Django, or PHP.

To implement caching, the `fastcgi_cache` is used with WordPress as an example. Key configurations include defining a cache path using `fastcgi_cache_path` in the HTTP context and setting `fastcgi_cache_key` for unique request identification. Caching parameters like `inactive` and `max_size` control cache eviction based on inactivity and maximum cache size, respectively.

By adding a custom response header, such as `X-Cache-Status`, the cache status (HIT or MISS) can be monitored. When a request is cached, subsequent requests for the same content are served faster, reducing the response time significantly.

WordPress intelligently uses response headers to control caching based on user login status. When logged in, headers like `Cache-Control: no-cache` are sent, preventing caching of dynamic content. For manual cache control, custom variables and conditionals can be used to bypass or prevent caching in specific scenarios, like for admin pages.

The setup also touches on limitations of the open-source NGINX versus NGINX Plus, such as the availability of cache purge functionality only in the commercial version. Lastly, it sets the stage for discussing microcaching in future sessions, which involves setting very short cache times to handle frequently changing content effectively.