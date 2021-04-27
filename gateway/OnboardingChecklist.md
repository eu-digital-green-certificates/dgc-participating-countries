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
