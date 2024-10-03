# Environment 
Docker plugin install 

# Start
docker-compose up -d

# Self Signed Certificates
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout prometheus.key -out prometheus.crt


# Reboot
cd Code
docker-compose down
docker-compose up -d