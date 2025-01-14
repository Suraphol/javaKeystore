# JavaKeystore
## กรณีมีเฉพาะไฟบ์ .pem

### แปลงไฟล์ .pem เป็น .p12

```bash
openssl pkcs12 -export -in certificate.pem -inkey private_key.pem -out certificate.p12 -name "your_alias" -password pass:you_password
```

### นำไฟล์ .p12 import Java keystore

```bash
keytool -importkeystore -srckeystore certificate.p12 -srcstoretype PKCS12 -srcalias "your_alis" -destkeystore keystore.jks -deststoretype JKS -deststorepass keystore_password -destalias "your_alias"

```
