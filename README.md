# JavaKeystore
## กรณีมีเฉพาะไฟล์ .pem

### แปลงไฟล์ .pem เป็น .p12

```bash
openssl pkcs12 -export \
-in certificate.pem \
-inkey private_key.pem \
-out certificate.p12 \
-name "your_alias" \
-password pass:you_password
```

### import p12 to Java keystore

```bash
keytool -importkeystore \
-srckeystore certificate.p12 \
-srcstoretype PKCS12 \
-srcalias "your_alis" \
-destkeystore keystore.jks \
-deststoretype JKS \
-deststorepass keystore_password \
-destalias "your_alias"
```

## กรณีมีทั้ง certificate และ private key แยกกัน

### รวมไฟล์ certificate และ private key
```bash
cat certificate.pem private_key.pem > combined.pem
```

### แปลงเป็น .p12
```bash
openssl pkcs12 -export \
-in combined.pem \
-out certificate.p12 \
-name "your_alias"
```

### import p12 to java keystore
```bash
keytool -importkeystore \
-srckeystore certificate.p12 \
-srcstoretype PKCS12 \
-destkeystore keystore.jks \
-deststoretype JKS
```

## กรณีมีเฉพาะ certificate .pem

### นำเข้า certificate โดยตรง
```bash
keytool -importcert \
-alias "your_alias" \
-file certificate.pem \
-keystore keystore.jks \
-storepass keystore_password
```

### ตรวจสอบการนำเข้า keystore
```bash
keytool -list -v -keystore keystore.jks
```


## Download Certificate จากเว็บไฟต์
```bash
openssl s_client -showcerts -connect url:port < /dev/null | openssl x509 -outform PEM > [filename.cert]
```

## นำ certificate ที่ download มาเข้า JavaStore
```bash
keytool -importcert -file [filename.cert] -alias [aliasName] -keystore $JAVA_HOME/lib/security/cacerts -storepass changeit
```
## ตรวจสอบ cert หลังนำเข้า
```bash
keytool -list -v -alias [alias_name] -cacerts - storepass changeit

# or

keytool -list -keystore $JAVA_HOME/lib/security/cacerts -storepass changeit | grep [alias_name]
```
## ลบ certificate 
```bash
keytool -delete -alias [name_alias] -keystore $JAVA_HOME/lib/security/cacerts -storepass changeit
```
