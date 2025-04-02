# Sample setup repo for docker-compose



### Generate self-signed certs

```powershell
# Set ssl config file
$configFile = "certs/openssl_config.conf"
# Generate CA Private Key
openssl genpkey -algorithm RSA -out certs/ca.key -pkeyopt rsa_keygen_bits:4096

# Generate CA Self-Signed Certificate
openssl req -x509 -new -nodes -key certs/ca.key -sha256 -days 7300 -out certs/ca.crt -subj "/CN=MyLocalCA"

# Generate Server Private Key
openssl genpkey -algorithm RSA -out certs/tls.key -pkeyopt rsa_keygen_bits:4096

# Generate Server Certificate Signing Request (CSR)
openssl req -new -key certs/tls.key -out certs/tls.csr -subj "/CN=*.local" -config $configFile

# Sign the Server Certificate using the CA
openssl x509 -req -in certs/tls.csr -CA certs/ca.crt -CAkey certs/ca.key -CAcreateserial -out certs/tls.crt -days 365 -sha256 -extfile $configFile

#Verify certificate chain
openssl verify -CAfile certs/ca.crt certs/tls.crt
```