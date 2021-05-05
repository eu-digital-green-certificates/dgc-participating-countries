# Work in progress!! (to be aligned with the final certificate governance document)

#  CSCA certificate (NB<sub>CSCA</sub>) generation example:

This guide follows the certificate templates defined [here](https://github.com/eu-digital-green-certificates/dgc-overview/blob/main/guides/certificate-governance.md).

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

# further example configuration files

## CSCA Conf
```
[ csca_ext ]
basicConstraints        = critical,CA:true,pathlen:0
keyUsage                = critical,keyCertSign,cRLSign
subjectKeyIdentifier    = hash
authorityKeyIdentifier  = keyid:always
issuerAltName           = dirName:dir_sect
subjectAltName          = dirName:dir_sect
crlDistributionPoints   = URI:http://crl.publichealth.xx/CRLs/XX-Health.crl
2.5.29.16               = ASN1:SEQUENCE:CAprivateKeyUsagePeriod


[ CAprivateKeyUsagePeriod ]
notBefore               = IMPLICIT:0,GENERALIZEDTIME:$ENV::PRIV_KEY_START
notAfter                = IMPLICIT:1,GENERALIZEDTIME:$ENV::CA_PRIV_KEY_END

[dir_sect]
L=XX
```
## DSC conf
```
[ document_signer_all_ext ]
keyUsage                = critical,digitalSignature
subjectKeyIdentifier    = hash
authorityKeyIdentifier  = keyid:always
subjectAltName          = dirName:dir_sect
issuerAltName           = dirName:dir_sect
crlDistributionPoints   = URI:http://crl.npkd.nl/CRLs/NLD-Health.crl
extendedKeyUsage        = 1.3.6.1.4.1.0.1847.2021.1.1,1.3.6.1.4.1.0.1847.2021.1.2,1.3.6.1.4.1.0.1847.2021.1.3
2.5.29.16               = ASN1:SEQUENCE:DSprivateKeyUsagePeriod

[ document_signer_test_ext ]
keyUsage                = critical,digitalSignature
subjectKeyIdentifier    = hash
authorityKeyIdentifier  = keyid:always
subjectAltName          = dirName:dir_sect
issuerAltName           = dirName:dir_sect
crlDistributionPoints   = URI:http://crl.npkd.nl/CRLs/NLD-Health.crl
extendedKeyUsage        = 1.3.6.1.4.1.0.1847.2021.1.1
2.5.29.16               = ASN1:SEQUENCE:DSprivateKeyUsagePeriod

[ document_signer_vaccinations_ext ]
keyUsage                = critical,digitalSignature
subjectKeyIdentifier    = hash
authorityKeyIdentifier  = keyid:always
subjectAltName          = dirName:dir_sect
issuerAltName           = dirName:dir_sect
crlDistributionPoints   = URI:http://crl.npkd.nl/CRLs/NLD-Health.crl
extendedKeyUsage        = 1.3.6.1.4.1.0.1847.2021.1.2
2.5.29.16               = ASN1:SEQUENCE:DSprivateKeyUsagePeriod

[ document_signer_recovery_ext ]
keyUsage                = critical,digitalSignature
subjectKeyIdentifier    = hash
authorityKeyIdentifier  = keyid:always
subjectAltName          = dirName:dir_sect
issuerAltName           = dirName:dir_sect
crlDistributionPoints   = URI:http://crl.npkd.nl/CRLs/NLD-Health.crl
extendedKeyUsage        = 1.3.6.1.4.1.0.1847.2021.1.3
2.5.29.16               = ASN1:SEQUENCE:DSprivateKeyUsagePeriod

[ DSprivateKeyUsagePeriod ]
notBefore               = IMPLICIT:0,GENERALIZEDTIME:$ENV::PRIV_KEY_START
notAfter                = IMPLICIT:1,GENERALIZEDTIME:$ENV::DS_PRIV_KEY_END
```
