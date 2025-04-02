# Edit Custom Page URL in Theme RemUI

## Feature Name
Custom Page URL Configuration

## Overview
The **Edit Custom Page URL** feature allows administrators to configure clean and user-friendly URLs for custom pages created in Edwiser RemUI. This is done by modifying server configurations for either Nginx or Apache, depending on the server being used.

## How to Use

### Step 1: Identify Your Server (Nginx or Apache)

Before making changes, you need to determine whether your Moodle site is running on an Nginx or Apache server.

#### How to Check Your Server Type:
- **Moodle Admin Panel:** Go to **Site administration > Server > PHP info** and look for the **Server API** section.
    - If it says **Apache**, you are using Apache.
    - If it mentions **FPM/FastCGI**, you're likely using Nginx.
  
- **Command Line (for those with server access):**
    - Run `apache2 -v` to check for Apache.
    - Run `nginx -v` to check for Nginx.

### Step 2: Configure Custom URLs

#### For Nginx Users
1. Open your Nginx configuration file located at `/etc/nginx/sites-available/default`.
2. Add the following code inside the **server block**:
    ```nginx
    location = /custom-page {
        rewrite ^/custom-page$ /local/edwiserpagebuilder/page.php?id=5 last;
    }
    ```
3. Save the file and restart Nginx:
    ```bash
    sudo systemctl restart nginx
    ```
    Now, the custom page will be available at:
    ```
    http://example.moodle.com/custom-page
    ```

##### For Multiple Pages:
Use the following configuration:
```nginx
location ~ ^/custom-page/(\d+)$ {
    rewrite ^/custom-page/(\d+)$ /local/edwiserpagebuilder/page.php?id=$1 last;
}
```
Now, you can access pages dynamically like:
```
http://example.moodle.com/custom-page/5
```

#### For Apache Users
1. Enable the mod_rewrite module:
    ```bash
    sudo a2enmod rewrite
    sudo systemctl restart apache2
    ```
2. Ensure the `.htaccess` file is in `/var/www/html/moodle/.htaccess`.
3. Add the following rule to your `.htaccess` file:
    ```apache
    RewriteEngine On
    RewriteRule ^custom-page$ /local/edwiserpagebuilder/page.php?id=5 [L]
    ```
4. Restart Apache:
    ```bash
    sudo systemctl restart apache2
    ```
    Now, your page will be available at:
    ```
    http://example.moodle.com/custom-page
    ```

##### For Multiple Pages:
Use the following configuration:
```apache
RewriteEngine On
RewriteRule ^custom-page/([0-9]+)$ /local/edwiserpagebuilder/page.php?id=$1 [L]
```
Now, pages will be accessible at:
```
http://example.moodle.com/custom-page/5


### Final Step: Restart Your Server
- **For Nginx:**
    ```bash
    sudo systemctl restart nginx
    ```
- **For Apache:**
    ```bash
    sudo systemctl restart apache2
    ```

### Notes:
- These changes enable clean, SEO-friendly URLs for custom pages.
- Be sure to restart your server after editing the configuration.

Now your custom pages should be accessible via their new clean URLs!
```

This is the entire **markdown** code for **editing custom page URLs** in **Edwiser RemUI**. You can copy and paste it directly into your markdown file to use it as documentation.

[Source: Edwiser RemUI Theme Documentation](https://remui-docs.edwiser.org/custom-page-creation/edit-custom-page-url)

This feature demonstrates Theme RemUI's commitment to providing professional URL management capabilities, enabling administrators to create clean and SEO-friendly URLs for their custom pages while maintaining compatibility with different server configurations. 
