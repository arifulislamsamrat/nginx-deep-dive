# Preparing a Node.js Sample Application English

## Reverse Proxy Section Overview

- **Focus:** Proxy traffic over HTTP using `proxy_pass` and associated directives.
- **Alternative Protocols:** FastCGI (for PHP servers), uWSGI (for Python applications).
- **Practical Setup:** Companion videos for setting up the software stack.

## Example Application Deployment

### Node.js Application: s3photoapp

- **Components:** Three web applications handling photo uploads, manipulation, and S3 integration.
- **Steps:**
    1. **Node.js Setup:**
        - Install Node.js on CentOS (version 8 LTS).
        - Command: `yum install nodejs`
    2. **Project Setup:**
        - Create directory `/srv/www` for web application code.
        - Clone the repository and navigate to the directory.
        - Run `make install` to install Node.js dependencies.

### AWS Setup

- **Services:** S3 and DynamoDB.
- **IAM User:**
    - Create a new IAM user `s3photos` with programmatic access.
    - Assign `S3FullAccess` and `DynamoDBFullAccess` permissions.
    - Note the access key ID and secret access key for configuration.

### Systemd Service Configuration

- **File Creation:** `/etc/systemd/system/photo-storage.service`
    - **Unit Section:**
        - Description: Photo-storage Node.js service.
        - Requires network access.
    - **Service Section:**
        - Restart policy: `always`
        - User and group: `nobody`
        - Environment variables: `NODE_ENV=production`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`
        - Execute start command: `/bin/node /srv/www/s3photoapp/apps/photo-storage/server.js`
    - **Install Section:**
        - Configure and enable the service.

### Similar Services Setup

- **Duplicate and Modify:** Create similar service files for `photo-filter` and `web-client`.
    - **photo-filter.service:** Change references from storage to filter, remove AWS credentials.
    - **web-client.service:** Change references to web-client and adjust the start command.

### Service Management

- **Enable and Start Services:**
    - `photo-storage`: `systemctl enable photo-storage && systemctl start photo-storage`
    - `photo-filter`: `systemctl enable photo-filter && systemctl start photo-filter`
    - `web-client`: `systemctl enable web-client && systemctl start web-client`
- **Check Status:**
    - Verify all services are running correctly using `systemctl status` commands.