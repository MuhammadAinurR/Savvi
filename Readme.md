# Backup and Restore Guide for WordPress and Database

## Backup WordPress Files

```bash
docker run --rm -v savvi_wordpress_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -czvf /backup/wordpress_data.tar.gz ."
```

## Backup Database

```bash
docker run --rm -v savvi_db_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -czvf /backup/db_data.tar.gz ."
```

## Restore WordPress Files

```bash
docker run --rm -v savvi_wordpress_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/wordpress_data.tar.gz"
```

## Restore Database

```bash
docker run --rm -v savvi_db_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/db_data.tar.gz"
```

---

# Setting Up a New Machine

1. Update the system and install Docker:
    ```bash
    sudo apt update
    sudo apt install -y docker.io
    sudo systemctl enable docker
    sudo systemctl start docker
    ```

2. Install Docker Compose:
    ```bash
    sudo apt install -y docker-compose
    ```

3. Start the containers:
    ```bash
    sudo docker-compose up -d
    ```

4. Stop the containers:
    ```bash
    sudo docker-compose down
    ```

5. Restore WordPress files:
    ```bash
    sudo docker run --rm -v savvi_wordpress_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/wordpress_data.tar.gz"
    ```

6. Restore the database:
    ```bash
    sudo docker run --rm -v savvi_db_data:/volume -v "${PWD}:/backup" alpine sh -c "cd /volume && tar -xzvf /backup/db_data.tar.gz"
    ```

7. Restart the containers:
    ```bash
    sudo docker-compose up -d
    sudo docker-compose restart
    ```

---

# Changing the Home Address Path

## Step 1: Copy the `wp-config.php` File

Run the following command to copy the `wp-config.php` file from the container to your local machine:

```bash
sudo docker cp wordpress:/var/www/html/wp-config.php ./wp-config.php
```

## Step 2: Edit the `wp-config.php` File

Open the `wp-config.php` file with a text editor of your choice:

```bash
nano wp-config.php
```

Or use a graphical code editor like VSCode or Sublime.

## Step 3: Add the Site URL Definitions

Add the following lines above the line `/* That's all, stop editing! Happy publishing. */`:

```php
define('WP_HOME', 'http://103.150.196.34:8080');
define('WP_SITEURL', 'http://103.150.196.34:8080');
```

## Step 4: Copy the Modified `wp-config.php` File Back to the Container

Save the file and copy it back to the container:

```bash
sudo docker cp ./wp-config.php wordpress:/var/www/html/wp-config.php
```

## Step 5: Restart the WordPress Container

Restart the WordPress container to apply the changes:

```bash
sudo docker-compose restart
```