# ansible-reverse-proxy
This role sets up NGINX (with either a self-signed cert or Let's Encrypt TLS cert) as a reverse proxy in front of an application. Nginx will listen on port 443 and send traffic back to a port of your choice.  This useful if you want to listen on a well-known port without running your app as root.  Only tested on RHEL.  

I used [matthiaslundberg's](https://github.com/mattiaslundberg/) excellent [ansible-letsencrypt](https://github.com/mattiaslundberg/ansible-letsencrypt) role as the basis for role.

##### Instructions
1. Define the followin vars in **vars/main.yml**:
    `nginx_domain_name` - the DNS name used by the app
    `app_port`    - the port on which the application behind the reverse proxy will be listening
    `letsencrypt_email` - email address provided to Let's Encrypt
    `use_letsencrypt`   - set to **true** for Let's Encrypt cert; set to **false** for self-signed cert

##### Task Overview
1. Create user groups
2. Install EPEL repo
3. Install nginx
4. Configure SSL using Let's Encrypt (use_letsencrypt=true)
  4a. Install letsencrypt and create its directory
  4b. Remove default nginx config
  4c. Create dirs and copy over Let's Encrypt nginx configs
  4d. Request and create Let's Encrypt cert
  4e. Add cron job for automatic cert renewal
5. Configure SSL using self-signed cert (SSC)
  5a. Remove default nginx config
  5b. Create dirs and copy over nginx config files for self-signed cert
  5c. Install pip and pyopenssl
  5d. Generate self-signed cert using openssl


##### Assumptions
1. Target server runs a RHEL-based Linux distro 
2. The vars described under the above "Instructions" section are defined.

