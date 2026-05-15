# 29bburkle's Nginx Reverse Proxy Tutorial
Here is a tutorial on how to put a already port forwarded program onto your domain using NGINX and Certbot! This is assuming you are using Ubuntu Server 

First, install nginx

    sudo apt install nginx

Next, delete the default nginx conf file

    sudo rm /etc/nginx/sites-enabled/default

Then, create a file called programname.conf under /etc/nginx/sites-available and copy and paste this in it, Make sure to replace {DOMAIN} with your domain name and {PORT} with the port your program is listening on

    server {
        listen 80;
        listen [::]:80;
 
        server_name {DOMAIN};
        
        location / {
            proxy_pass http://127.0.0.1:{PORT};
            include proxy_params;

            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

Now symlink the file and restart nginx

    sudo ln -s /etc/nginx/sites-available/programname.conf /etc/nginx/sites-enabled/programname.conf
    sudo systemctl restart nginx

Next, make sure that the domain you entered is in your domain registrar

Now we install certbot

    sudo apt install certbot
    sudo apt install python3-certbot-nginx

Now run this command to make an ssl certificate under your domain name. (You do not need to edit the nginx conf file, certbot will handle it.)

    sudo certbot --nginx -d {DOMAIN}

Now go to your domain on another device, (you have to use a vpn if you are on the same network as the server with the program), and you should see your program. Hope this tutorial worked for you.
