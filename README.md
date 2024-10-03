Sure, let's update the README to focus solely on Prometheus and remove Thanos. Here's the revised guide:

### Proof of Concept Implementation of Prometheus in Codespaces with Certificates

### Step 1: Set Up Prometheus in Codespaces

1. **Create a New Codespace**:
   - Open your GitHub repository in Codespaces.

2. **Create Docker Compose File**:
   - Create a `docker-compose.yml` file in your project directory with the following content:

   ```yaml
   version: '3.7'

   services:
     prometheus:
       image: prom/prometheus:latest
       volumes:
         - ./prometheus.yml:/etc/prometheus/prometheus.yml
         - ./prometheus.crt:/etc/prometheus/prometheus.crt
         - ./prometheus.key:/etc/prometheus/prometheus.key
       ports:
         - "9090:9090"
       networks:
         - code_default

   networks:
     code_default:
       driver: bridge
   ```

3. **Create Prometheus Configuration File**:
   - Create a `prometheus.yml` file in your project directory with the following content:

   ```yaml
   global:
     scrape_interval: 15s
     evaluation_interval: 15s
     external_labels:
       cluster: 'mycluster'
       replica: 'myreplica'

   scrape_configs:
     - job_name: 'prometheus'
       static_configs:
         - targets: ['localhost:9090']

   tls_config:
     cert_file: /etc/prometheus/prometheus.crt
     key_file: /etc/prometheus/prometheus.key
   ```

4. **Start the Services**:
   - Run the following command to start the services:
     ```sh
     docker-compose up -d
     ```

### Step 2: Implement Certificates for Secure Communication

1. **Generate Certificates**:
   - Use OpenSSL to generate self-signed certificates. Run the following commands in your terminal:

     ```sh
     openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout prometheus.key -out prometheus.crt
     ```

2. **Update Prometheus Configuration**:
   - Ensure the `prometheus.yml` file includes the certificate paths:

   ```yaml
   global:
     scrape_interval: 15s
     evaluation_interval: 15s
     external_labels:
       cluster: 'mycluster'
       replica: 'myreplica'

   scrape_configs:
     - job_name: 'prometheus'
       static_configs:
         - targets: ['localhost:9090']

   tls_config:
     cert_file: /etc/prometheus/prometheus.crt
     key_file: /etc/prometheus/prometheus.key
   ```

3. **Update Docker Compose File**:
   - Ensure the `docker-compose.yml` file mounts the certificates:

   ```yaml
   version: '3.7'

   services:
     prometheus:
       image: prom/prometheus:latest
       volumes:
         - ./prometheus.yml:/etc/prometheus/prometheus.yml
         - ./prometheus.crt:/etc/prometheus/prometheus.crt
         - ./prometheus.key:/etc/prometheus/prometheus.key
       ports:
         - "9090:9090"
       networks:
         - code_default

   networks:
     code_default:
       driver: bridge
   ```

4. **Restart the Services**:
   - Restart the services to apply the new configuration:
     ```sh
     docker-compose down
     docker-compose up -d
     ```

### Step 3: Verify the Setup

1. **Access Prometheus**:
   - Open your browser and navigate to `https://localhost:9090` to access Prometheus. You might need to accept the self-signed certificate warning.

This setup should provide a secure Prometheus environment running in Codespaces. Let me know if you encounter any issues or need further assistance! 😊