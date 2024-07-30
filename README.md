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
