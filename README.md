Sure, let's walk through a proof of concept implementation of Prometheus with Thanos in Codespaces, including setting up certificates for secure communication. Here’s a step-by-step guide:

### Step 1: Set Up Prometheus and Thanos in Codespaces

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
       ports:
         - "9090:9090"
       networks:
         - code_default

     thanos-sidecar:
       image: quay.io/thanos/thanos:v0.35.1
       command:
         - sidecar
         - --tsdb.path=/var/prometheus
         - --prometheus.url=http://prometheus:9090
         - --grpc-address=0.0.0.0:10941
         - --http-address=0.0.0.0:10942
         - --log.level=debug
       volumes:
         - ./prometheus-data:/var/prometheus
       depends_on:
         - prometheus
       ports:
         - "10941:10941"
         - "10942:10942"
       networks:
         - code_default

     thanos-query:
       image: quay.io/thanos/thanos:v0.35.1
       command:
         - query
         - --http-address=0.0.0.0:9091
         - --store=thanos-sidecar:10941
         - --log.level=debug
       depends_on:
         - thanos-sidecar
       ports:
         - "9091:9091"
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
     openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout thanos.key -out thanos.crt
     ```

2. **Update Prometheus Configuration**:
   - Update the `prometheus.yml` file to include the certificate paths:

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
   - Update the `docker-compose.yml` file to mount the certificates:

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

     thanos-sidecar:
       image: quay.io/thanos/thanos:v0.35.1
       command:
         - sidecar
         - --tsdb.path=/var/prometheus
         - --prometheus.url=https://prometheus:9090
         - --grpc-address=0.0.0.0:10941
         - --http-address=0.0.0.0:10942
         - --log.level=debug
         - --cert-file=/etc/thanos/thanos.crt
         - --key-file=/etc/thanos/thanos.key
       volumes:
         - ./prometheus-data:/var/prometheus
         - ./thanos.crt:/etc/thanos/thanos.crt
         - ./thanos.key:/etc/thanos/thanos.key
       depends_on:
         - prometheus
       ports:
         - "10941:10941"
         - "10942:10942"
       networks:
         - code_default

     thanos-query:
       image: quay.io/thanos/thanos:v0.35.1
       command:
         - query
         - --http-address=0.0.0.0:9091
         - --store=thanos-sidecar:10941
         - --log.level=debug
         - --cert-file=/etc/thanos/thanos.crt
         - --key-file=/etc/thanos/thanos.key
       depends_on:
         - thanos-sidecar
       ports:
         - "9091:9091"
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
   - Open your browser and navigate to `https://localhost:9090` to access Prometheus.

2. **Access Thanos Query**:
   - Open your browser and navigate to `https://localhost:9091` to access Thanos Query.

This setup should provide a secure Prometheus and Thanos environment running in Codespaces. Let me know if you encounter any issues or need further assistance! 😊
