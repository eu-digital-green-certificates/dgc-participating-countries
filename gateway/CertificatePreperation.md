#  CSCA certificate (NB<sub>CSCA</sub>) generation example:

This guide follows the certificate templates defined [here](https://github.com/eu-digital-green-certificates/dgc-overview/blob/main/guides/certificate-governance.md#42csca-certificate-nbcsca).

## csca.conf
Create a new file called *csca.conf* and replace the dn entries with your country details.
```
[req]
prompt = no
default_md = sha256
distinguished_name = dn

[dn]
C = DE
ST = NRW
L = Bonn
O = MinistryOfTest
OU = DGCOperations
CN = CSCA_DGC_DE_01

[ext]
basicConstraints = critical, CA:TRUE, pathlen:0
keyUsage = critical, digitalSignature, cRLSign, keyCertSign
subjectKeyIdentifier = hash 
```
## certificate generation
Open a command line prompt in the folder where the *csca.conf* is located and use the following OpenSSL command to create the private key (*privkey.pem*) and the certificate (*pub.pem*) using NIST-p-256:
```
openssl req -x509 -new -days 1461 -newkey ec:<(openssl ecparam -name prime256v1) -extensions ext -keyout privkey.pem -nodes -out pub.pem -config csca.conf
```
