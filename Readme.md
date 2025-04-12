backup
# Backup WordPress files
docker run --rm -v savviwp_wordpress_data:/volume -v "${PWD}:/backup" alpine `sh -c "cd /volume && tar -czvf /backup/wordpress_data.tar.gz ."

# Backup Database
docker run --rm -v savviwp_db_data:/volume -v "${PWD}:/backup" alpine `sh -c "cd /volume && tar -czvf /backup/db_data.tar.gz ."

restore
docker run --rm -v savviwp_wordpress_data:/volume -v "${PWD}:/backup" alpine `sh -c "cd /volume && tar -xzvf /backup/wordpress_data.tar.gz"

docker run --rm -v savviwp_db_data:/volume -v "${PWD}:/backup" alpine `sh -c "cd /volume && tar -xzvf /backup/db_data.tar.gz"
#   S a v v i  
 