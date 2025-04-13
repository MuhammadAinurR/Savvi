# Backup WordPress files

docker run --rm -v savvi_wordpress_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -czvf /backup/wordpress_data.tar.gz ."

# Backup Database

docker run --rm -v savvi_db_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -czvf /backup/db_data.tar.gz ."

# Restore WordPress files

docker run --rm -v savvi_wordpress_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/wordpress_data.tar.gz"

# Restore Database

docker run --rm -v savvi_db_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/db_data.tar.gz"

setup new machine
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
sudo apt install -y docker-compose
sudo docker-compose up -d
sudo docker-compose down
sudo docker run --rm -v savvi_wordpress_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/wordpress_data.tar.gz"
sudo docker run --rm -v savvi_db_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/db_data.tar.gz"
sudo docker-compose up -d
sudo docker-compose restart

how to change the home address path
Step 1: Copy the wp-config.php file from the container to your local machine
Run this command to copy the wp-config.php file from the container to your local machine:

bash
sudo docker cp wordpress:/var/www/html/wp-config.php ./wp-config.php
This will copy the wp-config.php file from the WordPress container to your current directory on your host machine.

Step 2: Edit the wp-config.php file
Now, open the wp-config.php file with any text editor of your choice (like nano, vim, or any code editor):

bash
nano wp-config.php
Or use a graphical code editor (VSCode, Sublime, etc.).

Step 3: Add the Site URL Definitions
In the wp-config.php file, above the line:

/* That's all, stop editing! Happy publishing. */
Add:

define('WP_HOME', 'http://103.150.196.34:8080');
define('WP_SITEURL', 'http://103.150.196.34:8080');

Step 4: Copy the modified wp-config.php file back to the container
After saving the file, copy it back into the container:

bash
sudo docker cp ./wp-config.php wordpress:/var/www/html/wp-config.php

Step 5: Restart the WordPress container
Finally, restart the WordPress container to apply the changes:

bash
sudo docker-compose restart