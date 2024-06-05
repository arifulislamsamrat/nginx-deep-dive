# Generating Self-signed Certificates ( English )

In this video, we will take a break from modifying our NGINX configuration and focus on generating a self-signed SSL certificate. This is essential for setting up our server to handle HTTPS requests, which adds an important layer of security through SSL and TLS. Although self-signed certificates aren't considered secure by browser standards, they are useful for our configuration purposes.

First, as the root user, we need to create a directory to store these certificates. We'll use the command **`mkdir`** to make a directory at **`/etc/nginx/ssl`**.

Next, we will use the **`openssl`** utility to generate the certificate request. This utility should be installed by default. The command we need to run involves several flags:

1. **`req`** - indicates that we are making a certificate request.
2. **`x509`** - specifies that the certificate should be in the x509 format.
3. **`nodes`** - means we do not want to encrypt the output key.
4. **`days`** - sets the certificate's expiration period, which we'll set to one year.
5. **`newkey`** - specifies the type of key to generate; we'll use **`rsa`** with a size of 2048 bits.
6. **`keyout`** - defines the output location for the private key, which we'll place in **`/etc/nginx/ssl/private.key`**.
7. **`out`** - sets the output location for the public certificate, which we'll place in **`/etc/nginx/ssl/public.pem`**.

After running this command, you will be prompted to enter some information, such as your company's details. You can skip this if you're only creating a self-signed certificate.

Once this process is complete, you can check the **`/etc/nginx/ssl`** directory to confirm that both the private key and the public certificate files are there.

In the next video, we will use these files to configure our server to accept HTTPS requests.