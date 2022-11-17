# docker-drupal-lamp
Docker setup for Drupal

PHP Version: 8.0, 
Apache Version: Apache/2.4.54, 
Mysql: Latest

Step 1: Create a folder and clone "docker-drupal-lamp" repository

Step 2: Run command "docker-compose up -d" in same directory where you did clone.

Step 3: Create "www" fodler in same directory. This will be your working directory.

Step 4: Run "docker exec -it dockers bash" to enter apache-php container

Step 5: Run "composer create-project drupal/recommended-project ." to download fresh drupal codebase.

Step 6: Edit "000-default.conf" and change DocumentRoot to /var/www/html/web

Step 7: Exit from container and restart docker using docker-compose restart.

Step 8: Run "docker exec -it db bash" to enter mysql container

Step 9: Connect mysql "mysql -u root -ptest" and create database

Step 10: Browse localhost, you will redirect to Drupal installation page.
