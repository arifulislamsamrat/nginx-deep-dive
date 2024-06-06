# Difference between NGINX & NGINX+ english

Before we start working with NGINX, installing it, and configuring it, we will cover the differences between NGINX and NGINX Plus. Understanding these differences is essential as we might encounter them throughout the course.

### Core Features

The open-source version of NGINX includes many standard features, especially those related to HTTP and functioning as a reverse proxy. These features make the open-source version powerful right out of the box.

### Load Balancing

NGINX Plus offers additional load-balancing features not available in the open-source version. While basic load balancing involves routing traffic to different versions of the same application and modifying them, NGINX Plus enhances this functionality. If your load-balancing needs surpass the capabilities of the open-source version, considering an NGINX Plus license might be beneficial.

### Security Features

Both versions include several security features, but NGINX Plus offers advanced options like JWT Authentication and ModSecurity 3.0 web application firewall at an additional cost. Although ModSecurity can be configured in the open-source version, it involves a tedious process of compilation and setup, which NGINX Plus simplifies.

### High Availability and Clustering

High availability and clustering are not included in the open-source version of NGINX. While there are alternative methods to achieve high availability using NGINX within a larger architecture, these features are more streamlined in NGINX Plus.

### Media Delivery

NGINX Plus includes features for media delivery, such as live streaming, which are absent in the open-source version. If you need to broadcast live streams, the open-source version will not suffice.

### Administrative Features

NGINX Plus offers advanced administrative features like live activity monitoring and on-the-fly reconfiguration of the load balancer via an API. These features allow for real-time adjustments without reloading configurations, a significant advantage over the open-source version.

### Conclusion

While NGINX Plus includes several proprietary and advanced features that simplify and enhance the server's capabilities, the core features most users need are available in the open-source version. The open-source version is robust and capable, but for specific use cases requiring high availability, advanced load balancing, media delivery, and dynamic reconfiguration, NGINX Plus offers valuable enhancements. Throughout this course, we will primarily focus on the open-source version, noting only occasionally where NGINX Plus might offer additional benefits.