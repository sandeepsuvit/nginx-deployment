# nginx-deployment
Documentation to deploy application to nginx

## Install nginx
```
sudo apt update
sudo apt install nginx
```

## Check status of nginx
```
sudo service nginx status
```

## Create new configuation
1. Copy the default configuration
```
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/website.com
```
2. Disable the default configuration file by removing the symlink in `/etc/nginx/sites-enabled/`
```
unlink /etc/nginx/sites-enabled/default
```
3. Create the new symlink with the new conf `website.com`
```
sudo ln -s /etc/nginx/sites-available/website.com /etc/nginx/sites-enabled/
```
4. Modify the configuration by `sudo vi /etc/nginx/sites-available/website.com` and add the following change
```
server {
    listen 80;
    server_name _;

    root /var/www/html/website;
    index index.html;

    # write access and error logs to /var/log
    access_log /var/log/website_access.log;
    error_log /var/log/website_error.log;

    location / {
        try_files $uri $uri/ /index.html;
        expires 1d;
    }
}
```
5. Navigate to `/var/www/html` and create a folder `website`
6. Add user `www-data` for accessing the files from the folder `/var/www/html/website/`
```
sudo adduser ubuntu www-data
sudo chown -R www-data:www-data /var/www
sudo chmod -R g+rwX /var/www
```
7. Check if the nginx configuration are correct using `sudo nginx -t`
8. If everything is correct then reload the nginx configuration `sudo service nginx reload`
