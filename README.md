# Gitea-on-docker
## How to install gitea in docker

### Docker commands to just run the serivces

### NGINX

docker run -d --name nginx -p 80:80 -p 443:443 -v /etc/nginx/htpasswd:/etc/nginx/htpasswd -v 
/etc/nginx/vhost.d:/etc/nginx/vhost.d:ro -v /etc/nginx/certs:/etc/nginx/certs -v 
/var/run/docker.sock:/tmp/docker.sock:ro etopian/nginx-proxy

#### Firewall config

sudo ufw allow 80,443/tcp
sudo ufw allow 'OpenSSH'
sudo ufw enable

### MySQL

docker run -d --name mysql-gitea -e MYSQL_ROOT_PASSWORD=o$su876HG@zvsRt3BT -v 
/opt/docker-volume/mysql-gitea:/var/lib/mysql mysql:5.7

#### MySQL configuration

docker container exec -it mysql-gitea bash

mysql -u root -po$su876HG@zvsRt3BT

mysql> CREATE USER 'gitea-user'@'%' IDENTIFIED BY '34@zv$TKji@s097BB';
mysql> CREATE DATABASE giteadb;
mysql> GRANT ALL PRIVILEGES ON giteadb.* TO 'gitea-user'@'%';
mysql> FLUSH PRIVILEGES;
mysql> exit;

exit

### Gitea 

docker run -d --name gitea-selfhosted -v /opt/docker-volume/gitea-selfhosted:/data -p 3000:3000 -e 
VIRTUAL_HOST=git.local -e VIRTUAL_PORT=3000 -e USER_UID=1001 -e USER_GID=1001 -e DB_TYPE=mysql -e 
DB_HOST=172.17.0.3:3306 -e DB_NAME=giteadb -e DB_USER=gitea-user -e DB_PASSWD=34@zv$TKji@s097BB 
gitea/gitea:1.8
