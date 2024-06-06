# Utilizing ModSecurity WAF English

### Securing Your Server with ModSecurity and OWASP Core Rule Set

In this video, we'll know how to secure our server from potential attacks using the ModSecurity dynamic module, which we have already installed, along with a core set of rules, specifically the OWASP ModSecurity Core Rule Set (CRS). This core set of rules handles a wide variety of common attacks, providing robust security against numerous threats.

### Importance of OWASP CRS

The OWASP CRS is invaluable for security as it is a collection of best practices compiled by experts. It addresses a wide range of vulnerabilities, making your server more secure. By using CRS, we leverage the knowledge and experience of security professionals who have contributed to making the web safer.

### Steps to Install and Utilize OWASP CRS

### Step 1: Setting Up the Environment

1. **Navigate to the NGINX Directory**:
    
    ```bash
    cd /etc/nginx
    ```
    
2. **Create ModSecurity Directory**:
    
    ```bash
    mkdir modsecurity
    cd modsecurity
    ```
    

### Step 2: Cloning the OWASP CRS Repository

1. **Clone the Repository**:
    
    ```bash
    git clone <https://github.com/SpiderLabs/owasp-modsecurity-crs.git>
    cd owasp-modsecurity-crs
    ```
    
2. **Review the Cloned Directory**:
    
    ```bash
    ls
    ```
    

### Step 3: Configuring OWASP CRS

1. **Copy Example Configuration Files**:
    
    ```bash
    cp crs-setup.conf.example crs-setup.conf
    cp rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
    cp rules/RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf.example rules/RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf
    ```
    
2. **Create an Includes Configuration File**:
    
    ```bash
    cd /etc/nginx/modsecurity
    touch modsecurity_includes.conf
    ```
    
3. **Add Basic Includes to `modsecurity_includes.conf`**:
    
    ```bash
    echo "Include /etc/nginx/modsecurity/modsecurity.conf" >> modsecurity_includes.conf
    echo "Include /etc/nginx/modsecurity/owasp-modsecurity-crs/crs-setup.conf" >> modsecurity_includes.conf
    ```
    
4. **Automatically Add Rule Includes**:
    
    ```bash
    for f in $(ls -1 /etc/nginx/modsecurity/owasp-modsecurity-crs/rules | grep -E '^(REQUEST|RESPONSE)-[0-9]{3}.*\\.conf$'); do
        echo "Include /etc/nginx/modsecurity/owasp-modsecurity-crs/rules/$f" >> modsecurity_includes.conf
    done
    ```
    

### Step 4: Configuring NGINX to Use the CRS

1. **Edit the NGINX Configuration for Your Site**:
    
    ```bash
    nano /etc/nginx/conf.d/your-site.conf
    ```
    
2. **Enable ModSecurity and Point to the Includes File**:
    
    ```
    server {
        # Other configurations
        modsecurity on;
        modsecurity_rules_file /etc/nginx/modsecurity/modsecurity_includes.conf;
    }
    ```
    
3. **Test and Reload NGINX Configuration**:
    
    ```bash
    nginx -t && systemctl reload nginx
    ```
    

### Step 5: Verifying ModSecurity

1. **Tail the ModSecurity Audit Log**:
    
    ```bash
    tail -f /var/log/nginx/modsec_audit.log
    ```
    
2. **Simulate an Attack**:
Open your website and append a cross-site scripting (XSS) attempt to the URL:
    
    ```
    <https://your-site.com/?param=><script>alert('XSS')</script>
    ```
    
3. **Check the Audit Log for Alerts**:
    
    ```bash
    tail -f /var/log/nginx/modsec_audit.log
    ```
    
    You should see an entry indicating that the XSS attempt was detected and blocked by ModSecurity.
    

So,we've configured ModSecurity with the OWASP Core Rule Set to enhance our server's security. This setup helps protect against a variety of common attacks, leveraging industry best practices. For further details and customization, refer to the well-documented files within the CRS repository. This hands-on approach to security ensures our applications are robust and less vulnerable to threats.