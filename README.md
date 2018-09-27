# hordeOnTumbelweed
A docker compose yml, which you can use to start a container with a running horde5 environment
This container is designed for interactive development and testing of libraries both from upstream and from custom projects.

You can run the command:
docker-compose -f docker-compose.yml up

Now you should be able to access the container with:

docker exec -it hordeOnTumbelweed_php_1 /bin/bash

Also you should be able to see the Horde environment via your browser on http://loacalhost 

To set up the database:

docker exec -it hordeOnTumbelweed_db_1 mysql -p -e "create database horde; grant all on horde.* to 'horde'@'%' identified by 'horde'";

docker exec -it hordeOnTumbelweed_php_1 chown wwwrun:www -R /srv/git/horde/base/config

Use the frontend for building the new configuration:
Go to the gear symbol select Administration->Configuration and then select the configuration for Horde.
    
Click on the database tab. Add the following settings:

SQL Database Settings:

$conf['sql']['phptype']    = MySQL/PDO
$conf['sql']['username']   = horde
$conf['sql']['password']   = horde
$conf['sql']['protocol']   = TCP/IP
$conf['sql']['hostspec']   = hordeOnTumbelweed_db_1
$conf['sql']['port']       = 3306
$conf['sql']['database']   = horde
$conf['sql']['charset']    = utf-8
$conf['sql']['ssl']        = No
$conf['sql']['splitread']  = Disabled
$conf['sql']['logqueries'] = no checkmark
    
Save the settings.
To finish the database setup run: 
docker exec -it hordeOnTumbelweed_php_1 /srv/git/horde/base/bin/horde-db-migrate
