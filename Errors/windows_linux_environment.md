Got it! Here are the steps to generate `.key` and `.crt` files using PowerShell, without the Docker Compose parts:

### Step 1: Generate a Self-Signed Certificate

1. **Open PowerShell as Administrator**:
   - Press `Win + X` and select `Windows PowerShell (Admin)`.

2. **Generate the Certificate**:
   - Run the following command to generate a self-signed certificate:
     ```powershell
     $cert = New-SelfSignedCertificate -DnsName "localhost" -CertStoreLocation "cert:\LocalMachine\My"
     ```

### Step 2: Export the Certificate and Key

1. **Export the Certificate and Key to a PFX File**:
   - Run the following command to export the certificate and key to a `.pfx` file:
     ```powershell
     $password = ConvertTo-SecureString -String "YourPassword" -Force -AsPlainText
     Export-PfxCertificate -Cert "cert:\LocalMachine\My\$($cert.Thumbprint)" -FilePath "C:\path\to\prometheus.pfx" -Password $password
     ```

2. **Convert PFX to PEM Format**:
   - If you need the certificate in PEM format, you can use OpenSSL (if available on another machine) or an online converter to convert the `.pfx` file to `.crt` and `.key` files. Alternatively, you can use PowerShell to extract the certificate and key.

### Step 3: Extract the Certificate and Key from the PFX File

1. **Install OpenSSL on Windows**:
   - If you have access to another machine with OpenSSL, you can use it to convert the `.pfx` file. If not, you can install OpenSSL on your Windows machine as described earlier.

2. **Convert PFX to PEM Using OpenSSL**:
   - Use the following commands to convert the `.pfx` file to `.crt` and `.key` files:
     ```sh
     openssl pkcs12 -in C:\path\to\prometheus.pfx -nocerts -out prometheus.key -nodes
     openssl pkcs12 -in C:\path\to\prometheus.pfx -clcerts -nokeys -out prometheus.crt
     ```

### Verify the Setup

1. **Inspect Certificate**:
   - You can inspect the certificate details using OpenSSL.
   ```sh
   openssl x509 -in C:\path\to\prometheus.crt -text -noout
   ```
   - This command will display the certificate details, including the issuer, subject, and validity period.

By following these steps, you can create `.key` and `.crt` files using PowerShell and verify their details. Let me know if you need any further assistance! 😊