# ghost
Ghost with docker-compose file(ghost, nginx, db)

git clone 
cd ghost
sudo docker-compose up -d


Error ตอนจะ import backup .json file
**"Request is larger than the maximum file size the server allows"**

Solve by editing **/etc/nginx/nginx.conf** by adding **"client_max_body_size 5M"** in the http section.

docker exec -it nginx /base
apt-get update
apt-get install vim

cd /etc/nginx
vim nginx.conf

Add "client_max_body_size 5M;" in the http section.





##nginx (default.conf)
server {
        listen 80;
        listen [::]:80;
  # redirect all HTTP to HTTPS
  server_name _;
  ##return 301 https://$host:4000$request_uri;
  return 301 https://$host$request_uri;
}

server {
  # SSL configuration
  listen 443 ssl;
  listen [::]:443 ssl;

  # self-ssl
  ssl_certificate /etc/ssl/localhost/server.cert;
  ssl_certificate_key /etc/ssl/localhost/server.pem;


  location  / {
      expires off;
      proxy_pass http://ghost:2368;
      proxy_set_header    X-Real-IP $remote_addr;
      proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header    Host $http_host;
      proxy_set_header    X-NginX-Proxy true;
      proxy_buffering     on;
      proxy_buffer_size   1k;
      proxy_buffers 24    4k;
      proxy_busy_buffers_size     8k;
      proxy_max_temp_file_size    2048m;
      proxy_temp_file_write_size  32k;
  }

  location ~ /.well-known/acme-challenge/ {
      allow all;
      root /var/www/certbot;
  }
}




##run .init-letsencrypt.sh
give file permission before run the script. brows to script directory.
sudo chmod +x init-letsencrypts.sh
./init-letsencrypts.sh

##run sudo docker-compose up -d

nginx restarting > rerun .sh and docker-compose (wait a little all will running).


