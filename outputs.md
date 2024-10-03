# EXPECTATION SEE IT WORKING
![image](https://github.com/user-attachments/assets/bd93c0f2-2ad7-4f73-b10a-cd4482ee8448)


# CERTIFICATES
![alt text](/Resources/created_certificates.png)


# VERIFY CERTIFICATES
To verify the implementation of certificates in your Prometheus setup, you can follow these steps:

### Step 1: Check Prometheus Logs
1. **View Logs**:
   - Check the Prometheus container logs to ensure it started correctly with the certificates.
   ```sh
   docker logs <prometheus_container_id>
   ```
   - Look for messages indicating that the certificates were loaded successfully.

### Step 2: Access Prometheus via HTTPS
1. **Open Browser**:
   - Open your web browser and navigate to `https://localhost:9090`.
   - You should see a warning about the self-signed certificate. This is expected.

2. **Accept the Certificate**:
   - Proceed to accept the self-signed certificate warning. The exact steps depend on your browser:
     - **Chrome**: Click on "Advanced" and then "Proceed to localhost (unsafe)".
     - **Firefox**: Click on "Advanced" and then "Accept the Risk and Continue".
     - **Edge**: Click on "Details" and then "Go on to the webpage (Not recommended)".

3. **Verify Secure Connection**:
   - Once you accept the certificate, you should be able to access the Prometheus web interface.
   - Verify that the connection is secure by checking for the padlock icon in the browser's address bar.

### Step 3: Use cURL to Test HTTPS Endpoint
1. **Run cURL Command**:
   - Use `cURL` to make a request to the Prometheus HTTPS endpoint and verify the certificate.
   ```sh
   curl -v --cacert ./Cert/prometheus.crt https://localhost:9090
   ```
   - This command uses the `--cacert` option to specify the CA certificate to verify the server's certificate.

2. **Check Response**:
   - Ensure you receive a valid response from Prometheus. The output should include details about the SSL handshake and the HTTP response.

### Step 4: Verify Certificate Details
1. **Inspect Certificate**:
   - You can inspect the certificate details using OpenSSL.
   ```sh
   openssl x509 -in ./Cert/prometheus.crt -text -noout
   ```
   - This command will display the certificate details, including the issuer, subject, and validity period.

### Step 5: Check Prometheus Configuration
1. **Verify Configuration**:
   - Ensure the `prometheus.yml` file includes the correct paths to the certificate and key files.
   ```yaml
   tls_config:
     cert_file: /etc/prometheus/prometheus.crt
     key_file: /etc/prometheus/prometheus.key
   ```

By following these steps, you can verify that the certificates are correctly implemented and that Prometheus is securely accessible via HTTPS. Let me know if you need any further assistance! 😊

Public Key
![alt text](/Resources/see-certificate.png)

To verify the implementation of certificates in your Prometheus setup, you can follow these steps:

### Step 1: Check Prometheus Logs
1. **View Logs**:
   - Check the Prometheus container logs to ensure it started correctly with the certificates.
   ```sh
   docker logs <prometheus_container_id>
   ```
   - Look for messages indicating that the certificates were loaded successfully.

### Step 2: Access Prometheus via HTTPS
1. **Open Browser**:
   - Open your web browser and navigate to `https://localhost:9090`.
   - You should see a warning about the self-signed certificate. This is expected.

2. **Accept the Certificate**:
   - Proceed to accept the self-signed certificate warning. The exact steps depend on your browser:
     - **Chrome**: Click on "Advanced" and then "Proceed to localhost (unsafe)".
     - **Firefox**: Click on "Advanced" and then "Accept the Risk and Continue".
     - **Edge**: Click on "Details" and then "Go on to the webpage (Not recommended)".

3. **Verify Secure Connection**:
   - Once you accept the certificate, you should be able to access the Prometheus web interface.
   - Verify that the connection is secure by checking for the padlock icon in the browser's address bar.

### Step 3: Use cURL to Test HTTPS Endpoint
1. **Run cURL Command**:
   - Use `cURL` to make a request to the Prometheus HTTPS endpoint and verify the certificate.
   ```sh
   curl -v --cacert ./Cert/prometheus.crt https://localhost:9090
   ```
   - This command uses the `--cacert` option to specify the CA certificate to verify the server's certificate.

2. **Check Response**:
   - Ensure you receive a valid response from Prometheus. The output should include details about the SSL handshake and the HTTP response.

### Step 4: Verify Certificate Details
1. **Inspect Certificate**:
   - You can inspect the certificate details using OpenSSL.
   ```sh
   openssl x509 -in ./Cert/prometheus.crt -text -noout
   ```
   - This command will display the certificate details, including the issuer, subject, and validity period.

### Step 5: Check Prometheus Configuration
1. **Verify Configuration**:
   - Ensure the `prometheus.yml` file includes the correct paths to the certificate and key files.
   ```yaml
   tls_config:
     cert_file: /etc/prometheus/prometheus.crt
     key_file: /etc/prometheus/prometheus.key
   ```

By following these steps, you can verify that the certificates are correctly implemented and that Prometheus is securely accessible via HTTPS. Let me know if you need any further assistance! 😊

# Checking on the command line
![alt text](/Resources/check-oncommandline.png-1.png)