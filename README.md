# Basic-nginx-reverse-proxy-config
A sample nginx reverse proxy config for those who need it. Make sure to replace {DOMAIN} with your domain/subdomain and {PORT} with the port that your program is listening on

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


TIP: If you are using certbot, it will automatically do the ssl cert stuff
