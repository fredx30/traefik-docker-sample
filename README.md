# Sample setup repo for docker-compose



### Generate self-signed certs

```powershell
# Generate a private key
openssl genpkey -algorithm RSA -out certs/tls.key -pkeyopt rsa_keygen_bits:4096
# Generate a self-signed certificate
openssl req -x509 -new -nodes -key certs/tls.key -sha256 -days 365 -out certs/tls.crt -subj "/CN=*.local"  -addext "subjectAltName = DNS:localhost, DNS:127.0.0.1, DNS::1, DNS:*.local"
```