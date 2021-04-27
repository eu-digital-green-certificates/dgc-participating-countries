# Onboarding Checklist

For a successfull connection to the gateway there are several steps to prepare: 

 1) Certificates must be prepared for Test Environment
    - Authentication: NB<sub>TLS</sub>
    - Upload:   NB<sub>UP</sub>
    - CSCA(s):  NB<sub>CSCA</sub>
 2) Send the Public Keys in CRT Format to the contact of the Test Operator
 3) After Onboarding in the Test Environment, check the connectitivy with the following command:<br>
  ```curl -X GET https://*****.ec.europa.eu/trustlist --cert my_client_cert.crt --key my_private_key.pem``` <br>
    You should see a output like: <br>
    ![TrustListOutput](./../images/TrustListResult.PNG)
 4) Test the other Truslist Routes in the same style (e.g. with DSC/CSCA/Upload/Authentication...)
 5) Create an Document Signer Certificate and sign it by the CSCA
 6) Create an CMS Package with the following Command: 
  ``` 
      openssl x509 -outform der -in cert.pem -out cert.der
      openssl cms -sign -nodetach -in cert.der -signer signing.crt -inkey signing.key -out signed.der -outform DER -binary
      openssl base64 -in signed.der -out signed.b64 -e -A 
  ``` 
   Note: cert.der is your DSC, signing.crt ist the Uploader Certificate)
  
 7) Upload the CMS Package to the Gateway<br>
    ```curl -X POST -H "Content-Transfer-Endcoding: base64" -H "Content-Type: application/cms" https://*****.ec.europa.eu/signercertificate --cert my_client_cert.crt --key my_private_key.pem  --data cms.b64``` <br>
    

