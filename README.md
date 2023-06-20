# assignments
assignment-submission


Question 1)	Check if docker and docker-compose is installed on the system. If not present, install the missing packages.
Installing docker and:

•	Launching a ec2-instance & login into server

•	yum install docker -y

•	using above command installed docker 

•	usermod -aG docker ec2-user – docker added into ec2-user groupinstalling docker compose:

•	DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}

•	mkdir -p $DOCKER_CONFIG/cli-plugins

•	curl -SL https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
apply executable permissions

•	chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose

•	docker compose version – to check docker compose available or not



Question 6&7) Add another subcommand to enable/disable/delete the site (stopping/starting the containers)

•	created a project directory mkdir my-lemp-stack

•	cd my-lemp-stack

•	in this directory, empty file created using touch docker_delete.sh and given the execution permissions usinf chmod +x docker_delete.sh

•	./docker_delete.sh using this command executed the script
      
      
     
#!/bin/bash

start_site() {

   echo "Starting containers for the site..."

   docker compose up -d

}

stop_site() {

   echo "Stopping containers for the site..."
  
   docker compose down

}

delete_site() {

   echo "Deleting the site..."
   
   stop_site

   rm -rf /path/to/site

}

enable_site() {

   echo "Enabling the site..."
   
   start_site

}

disable_site() {

   echo "Disabling the site..."
   
   stop_site

}

case "$1" in

   enable)
   
      enable_site
      
      ;;
   
   disable)
   
      disable_site
      
      ;;
   
   delete)
   
      delete_site
      
      ;;
   
   *)

echo "Usage: $0 {enable|disable|delete}"
 
      exit 1

esac



Question2) The script should be able to create a WordPress site using the latest WordPress Version. Please provide a way for the user to provide the site name as a command-line argument.

•	Installed php, httpd & mariadb

•	Yum install httpd php -y this command used to install php and httpd 

•	Steps to Installing mariadb

•	wget https://r.mariadb.com/downloads/mariadb_repo_setup

•	echo "3a562a8861fc6362229314772c33c289d9096bafb0865ba4ea108847b78768d2 mariadb_repo_setup"     | sha256sum -c -

•	chmod +x mariadb_repo_setup

•	sudo ./mariadb_repo_setup    --mariadb-server-version="mariadb-10.3"

•	yum install MariaDB-server MariaDB-client -y



cd /etc/yum.repo.d/ in this path we can see tha mariadb repo

•	created a file vi create_wp_site.sh


#!/bin/bash

if [ -z "$1" ]; then

  echo "Error: Site name not provided. Usage: create_wp_site.sh <site-name>"
  
  exit 1

fi

# Variables

site_name="$1"

wordpress_dir="/var/www/$site_name"

db_name="$site_name"

db_user="tom"     

db_password="tomcat" 

# Download the latest WordPress version

latest_version=$(curl -s https://wordpress.org/latest.tar.gz | tar -xzv -C /var/www/)

# Create a new MySQL database for the WordPress site

mysql -uroot -p -e "CREATE DATABASE ${anand};"

# Configure file permissions

chown -R cat:cat $wordpress_dir

chmod -R 755 $wordpress_dir

# Create a wp-config.php file

cp $wordpress_dir/wp-config-sample.php $wordpress_dir/wp-config.php

# Update database connection details in wp-config.php

sed -i "s/database_name_here/${anand}/" $wordpress_dir/wp-config.php

sed -i "s/username_here/${tom}/" $wordpress_dir/wp-config.php

sed -i "s/password_here/${tomcat}/" $wordpress_dir/wp-config.php

echo "WordPress site '$site_name' created successfully!"


      •	useradd cat ; passwd tom & chown -R cat:cat /var/www/wordpress using this command given the ownership of file


      •	created a user and given password to it

      
      •	./create_wp_site.sh

      
      
question 4) Create a /etc/hosts entry for example.com pointing to localhost. Here we are assuming the user has provided example.com as the site name.

•	Created a server and login into terminal

•	Changed ec2-user home directory to etc directory

•	In etc we have hosts file. Opened it using vi hosts

•	In hosts file given my private Ip of server and url

Example 172.31.17.210   https://www.example.com and saved it and exit the file

•	Ping 172.31.17.210 command used

     
      
question 5) Prompt the user to open example.com in a browser if all goes well and the site is up and healthy.

      IN etc folder created a file touch open_website.sh

•	Chmod +x open_website.sh

•	In this file  

•	#!/bin/bash

•	website="http://example.com"

•	# Check if the website is reachable

•	if curl --output /dev/null --silent --head --fail "$website"; then

•	  echo "The website $website is up and healthy."

•	  echo "Opening $website in a browser..."

•	  xdg-open "$website"

•	else

•	  echo "The website $website is not reachable or experiencing issues."

•	Fi

•	To prompt the user to open a website in a browser using linux, uses the xdg-open command

•	After installing the xdg-utils plugin I can use xdg-open 

•	To install xdg-utils I use yum install xdg-units 

      
      
question 3) 	It must be a LEMP stack running inside containers (Docker) and a docker-compose file is a must.

•	Created a project directory: mkdir my-lemp-stack

•	Cd My-lemp-stack & create a docker-compose.yml

•	In docker-compose.yml

version: '3'

services:

  
nginx:

    image: nginx:latest
    
    ports:
    
      - '8080:80'
    volumes:
      
      - ./nginx.conf:/etc/nginx/nginx.conf
      
      - ./html:/var/www/html
    
    depends_on:
    
      - php

  php:
  
    image: php:latest
    
    volumes:
    
      - ./html:/var/www/html

  mysql:
    
    image: mysql:latest
    
    restart: always
    
    environment:
    
      MYSQL_ROOT_PASSWORD: anand
    
    volumes:
    
      - ./mysql-data:/var/lib/mysql

Installation of mariaDB

wget https://r.mariadb.com/downloads/mariadb_repo_setup

echo "3a562a8861fc6362229314772c33c289d9096bafb0865ba4ea108847b78768d2 mariadb_repo_setup"     | sha256sum -c -

chmod +x mariadb_repo_setup

sudo ./mariadb_repo_setup    --mariadb-server-version="mariadb-10.3"

yum install MariaDB-server MariaDB-backup

installing php

yum install php -y

touch nginx.conf Created empty file

events {}


http {

  server {
  
    listen 80;
    
    root /var/www/html;
    
    index index.php index.html;

    location / {
    
      try_files $uri $uri/ =404;
    
    }

    location ~ \.php$ {
    
      try_files $uri =404;
      
      fastcgi_pass php:9000;
      
      fastcgi_index index.php;
      
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      
      include fastcgi_params;
    
    }
  
  }

}

      
docker compose up -d


