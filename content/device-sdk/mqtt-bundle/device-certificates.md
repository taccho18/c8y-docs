---
weight: 80
title: Device certificates
layout: redirect
---

### Overview

Devices can authenticate against the {{< product-c8y-iot >}} platform using X.509 client certificates.  

Devices can communicate using the MQTT interface of the platform, but MQTT over WebSocket is not supported. The {{< product-c8y-iot >}} platform expects devices to connect using SSL on port 8883.

Each tenant individually defines whom it trusts by uploading the base CA certificate.

Devices connecting to the platform with certificates do not need to provide the tenant ID, username and password. Authentication information will be obtained from the certificates.

#### General requirements for connecting devices with certificates

* The CA certificate may also be a self-signed certificate.
* Certificates must be uploaded as X.509 version 3 certificates.
* Uploaded certificates must have set `BasicConstraints:[CA:true]`.
* The certificate's common name should not contain `:` characters, see [MQTT ClientId](#mqtt-clientid) for more information.
* Devices must trust the {{< product-c8y-iot >}} server certificate.
* Certificates used by devices must contain the full certificate chain, including the uploaded CA certificate.
* Certificates used by devices must be signed either by uploaded CA certificates or by a chain of certificates signed by uploaded CA certificates.

### Registering devices using certificates

{{< product-c8y-iot >}} supports two ways to register devices which will be able to connect using certificates:

**Auto registration**

The user for the device will be created during the first MQTT call, if a device certificate is derived from a trusted certificate which was uploaded to the {{< product-c8y-iot >}} platform with a flag _autoRegistrationEnabled_ with a value of true.
Auto-registration needs to be activated for the uploaded certificate.
If auto-registration is not activated it is required to use the bulk registration (see below).
To manage the auto registration field of uploaded certificates in the UI refer to [Device Management > Managing device data > Managing trusted certificates](/users-guide/device-management#trusted-certificates).

**Bulk registration**

The user for the device can also be created via the standard [bulk registration](/users-guide/device-management/#to-bulk-register-devices) in Device Management.

The CSV file used in bulk registration should meet the requirements described in [Create a bulk device credentials request](https://{{< domain-c8y >}}/api/{{< c8y-current-version >}}/#operation/postBulkNewDeviceRequestCollectionResource) in the {{< openapi >}}. Moreover, it is required that the CSV file has an additional column AUTH_TYPE with value CERTIFICATES, and that the column CREDENTIALS is either not present or has an empty value.

**Single registration**

Single registration is not supported for devices which are going to use certificates for authentication.

{{< c8y-admon-info >}}
During device registration, the device user is created, which is necessary for device communication with the platform.
{{< /c8y-admon-info >}}

### JWT token retrieval

A device which is authenticated by certificates and connected to the {{< product-c8y-iot >}} platform can receive a token which can later be used to authenticate HTTP requests.

* First the device subscribes to the topic <kbd>s/dat</kbd>.
* Then the device publishes an empty message on the topic <kbd>s/uat</kbd>.
* After a while a token will be published on the subscribed <kbd>s/dat</kbd> topic in the format:

```plain
71,<<Base64 encoded JWT token>>
```

A device token lifetime can be configured using tenant options with a category of `oauth.internal` and a key of `device-token.lifespan.seconds`.
The default value is 1 hour.
The minimum allowed value is 5 minutes.
Refer to the [Tenant API](https://{{< domain-c8y >}}/api/{{< c8y-current-version >}}/#tag/Tenant-API) in the {{< openapi >}} for more details.

A device can fetch a new device token before the old one expires, if it request a JWT token after half of the token's lifetime has passed.

{{< c8y-admon-caution >}}
A device can only subscribe to a topic like <kbd>s/dat</kbd> once certificate based mutual authentication is successful. The MQTT broker will not make any information available on the device's subscribed topics until the device publishes a message to <kbd>s/uat</kbd> or <kbd>s/us</kbd>.
{{< /c8y-admon-caution >}}

### Introduction to X.509 certificates

X.509 is a standard that defines public key certificates, which are commonly used in the SSL protocol to provide secure connection and data transfer.
Version 3 is up-to-date since 1995.

The general purpose of an X.509 certificate is to bind an identity to a pair of keys: the public key is publicly known as a part of the certificate while the private key is known only by the certificate owner.
Such pairs of keys should be created with an asymmetric-key algorithm, which is considered secure nowadays.
An example of such an algorithm is RSA with at least 2048 bits key size.
A size of 1024 bits or lower is no longer considered safe.
A private key can be used in two ways:

* To prove that the message was sent by the certificate owner - The owner encrypts some message with his private key and sends it. Then the receiver can decrypt it with the sender's public key. If the decrypted message matches his expectation, then he can be sure that it was sent by the certificate owner.
* To read a message intended only for the certificate owner - If someone encrypts the message with receiver public key, then only the owner of the private key would be able to decrypt it. No third party who somehow intercepted the message will be able to read it.

Every certificate can be self-signed or can be signed by another certificate.
To tell if the certificate is self-signed you can look at the "Issuer Name" and "Subject Name" fields of the certificate.
If they are the same then it means that it is a self-signed certificate, otherwise the certificate claims to be signed by the issuer.
To verify if the issuer really signed the certificate, you must check the "Signature" field of the certificate.
After decryption with the issuer's public key, the signature should match the data of the signed certificate.
Signing a certificate by another, means that if the issuer's certificate is trusted then the signed certificate also can be trusted.

For example if a platform trusts the customer certificate and that customer has 20 devices with individual certificates, then he does not have to upload each one of them.

If these device certificates are signed by the customer certificate, then the platform should trust them too.
In this case, every device should send not only its own certificate, but the whole chain of certificates (so-called chain of trust) during the SSL handshake.

The chain of certificates starts with the one belonging to the device, through all used intermediate certificates until it reaches the CA certificate trusted by the platform.
Usually the chain of certificates does not have to contain the trusted CA certificate, so it can end with the certificate signed directly by the CA.
However, in the {{< product-c8y-iot >}} platform it is also required to provide the trusted CA certificate in the chain of certificates.

Providing the chain of certificates lets the platform verify the signatures of every certificate in the chain to make sure that the device certificate is signed directly or indirectly by the trusted certificate.
The chain of the certificates can differ in length, so if the platform trusts certificate A and certificate B is signed by A, and certificate C is signed by B, then certificate C will also be trusted.
However, there are a few things to keep in mind:

* Every certificate which is used to sign another certificate must contain the extension "CA:TRUE".
* The lengths of the chain of certificates can be restricted by the certificate extension "pathlen". This extension limits the amount of other CA certificates that can be placed in the chain between the device certificate and the one with that extension. For example the valid chain of the certificates with minimal values of path length would look like this: "A (CA:TRUE, pathlen: 2) -> B (CA:TRUE, pathlen: 1) -> C (CA:TRUE, pathlen: 0) -> D (device with CA:FALSE)"".

The structure of an X.509 certificate in version 3 looks like this:

* Version - The version number of the x.509 certificate,
* Serial Number - Unique serial number that is created for each certificate that is created by an issuer,
* Issuer - The distinguished name of the issuer,
* Not Before - Date since the certificate is valid,
* Not After - Date since the certificate is expired,
* Subject - Distinguished name of the certificate's owner,
* Public Key Algorithm - Algorithm used to generate the public key,
* Subject Public Key - Public key value,
* Certificate Signature Algorithm - Algorithm used to generate the certificate signature,
* Certificate Signature - Certificate signature generated by encrypting certificate data with the issuer's private key,
* Extensions - optional; they are responsible for providing various pieces of information, for example if the certificate is CA, which means that it can be used to sign another certificate. Added in version 3 of X.509,
* Issuer Unique Identifier - optional; can be present in the certificate to handle the possibility of reuse of issuer names,
* Subject Unique Identifier - optional; can be present in the certificate to handle the possibility of reuse of subject names.

To show how the authentication with X.509 certificates works, there is an example of the simplified mutual SSL handshake:

![Simplified mutual SSL handshake](/images/mqtt/sslMutualSimplified.png)

A server knows that the client is the owner of the certificate it sent, after the server has decrypted the encrypted copy of the message using the client's public key.
The client knows that the server is the owner of the certificate it sent, if the server uses the correct session key, because that means that the server decrypted it with its private key.
In basic authentication with username and password, the password must be sent over the network to authenticate the user.
When certificates are used, the private key is never sent, which makes the use of the certificates much more secure than the basic authentication with username and password.

There are 3 important terms related to X.509 certificates, which everyone should know who wants to start working with them:

* Keystore is a file which a device uses to authenticate itself in the mutual SSL handshake. That means that keystore contains the chain of the certificates used by the device and the private key of the device.
* Truststore is a file which contains all trusted certificates. A server or device would only establish a connection with something that is using a certificate from their truststore. When the chain of certificates is used, only one of the certificates in the chain must be trusted to establish the connection.
* Certificate Authority (CA) in the simplest sense is an entity that signs certificates.

Both keystore and truststore can be stored in the same file, but for security reasons we recommended you to keep them separately.
The truststore contains public data and the keystore contains the private key, which only to the owner should know.
The most popular formats for these files are:

* PKCS12 (Public Key Cryptography Standards, version 12), which can be generated using the OpenSSL toolkit.
* JKS (Java KeyStore), which can be generated with the Java Keytool.

### Generating and signing certificates

To generate certificates we use the OpenSSL toolkit. If you do not have it already installed, then you can download if from the website: https://www.openssl.org/source/

#### Creating a self-signed CA certificate

1. Create a directory for the root certificate and the signing configuration, for example:

    `mkdir /home/user/Desktop/caCertificate`

2. Go to the created directory and create a configuration file for your CA certificate:

    `touch caConfig.cnf`

3. Create a database file for keeping the history of certificates signed by the CA:

    `touch database.txt`

4. Create a serial file with initial serial number, which will be used to identify signed certificates. After assigning this serial to the signed certificate, the value in this file will be automatically incremented:

    `echo 1000 > serial`

5. Create subdirectories for signed certificates and the certificate revocation list:

    `mkdir deviceCertificates crl`

6. Fill in the configuration file. This is the example configuration, which can be used for tests after changing the directory `dir` to your own. If you want to use it in the production environment then please consult it first with some security specialist:

    ```text
    [ ca ]
    default_ca = CA_default
    [ CA_default ]
    # Directory and file locations.
    dir               = /home/user/Desktop/caCertificate
    certs             = $dir # directory where the CA certificate will be stored.
    crl_dir           = $dir/crl # directory where the certificate revocation list will be stored.
    new_certs_dir     = $dir/deviceCertificates # directory where certificates signed by CA certificate will be stored.
    database          = $dir/database.txt # database file, where the history of the certificates signing operations will be stored.
    serial            = $dir/serial # directory to the file, which stores next value that will be assigned to signed certificate.

    # The CA key and CA certificate for signing other certificates.
    private_key       = $dir/caKey.pem # CA private key which will be used for signing certificates.
    certificate       = $dir/caCert.pem # CA certificate, which will be the issuer of signed certificate.

    default_md        = sha256 # hash function
    default_days      = 375 # default number of days for which the certificate will be valid since the date of its generation.
    preserve          = no # if set to 'no' then it will determine the same order of the distinguished name in every signed certificate.
    policy            = signing_policy # the name of the tag in this file that specifies the fields of the certificate. The fields must be filled in or even match the CA certificate values to be signed.

    # For certificate revocation lists.
    crl               = $crl_dir/caCrl.pem # CA certificate revocation list
    crlnumber         = $crl_dir/crlnumber # serial, but for the certificate revocation list
    crl_extensions    = crl_ext # the name of the tag in this file, which specifies certificates revocation list extensions, which will be added to the certificate revocation by default.
    default_crl_days  = 30 # default number of days for which the certificate revocation list will be valid since the date of its generation. After that date it should be updated to see if there are new entries on the list.

    [ req ]
    default_bits        = 4096 # default key size in bits.
    distinguished_name  = req_distinguished_name # the name of the tag in this file, which specifies certificates fields description during certificate creation and eventually set some default values.
    string_mask         = utf8only # permitted string type mask.
    default_md          = sha256 # hash function.
    x509_extensions     = v3_ca # the name of the tag in this file, which specifies certificates extensions, which will be added to the created certificate by default.

    # descriptions and default values of the created certificate fields.
    [ req_distinguished_name ]
    countryName                     = Country Name (2 letter code)
    stateOrProvinceName             = State or Province Name
    localityName                    = Locality Name
    organizationName                = Organization Name
    organizationalUnitName          = Organizational Unit Name
    commonName                      = Common Name
    emailAddress                    = Email Address

    # A default value for each field can be set by adding an extra line with field name and postfix "_default". For example: "countryName_default = PL". If you add this line here, then leaving country name empty during certificate creation will result in the value "PL" being used. If the default value was specified there, but during certificate creation you do not want to use this value, then instead use "." as the value. It will leave the value empty and not use the default.

    # default extensions for the CA certificate.
    [ v3_ca ]
    subjectKeyIdentifier = hash # subject key value will be calculated using hash funtion. It's the recommended setting by PKIX.
    authorityKeyIdentifier = keyid:always,issuer # The subject key identifier will be copied from the parent certificate. It's the recommended setting by PKIX.
    basicConstraints = critical, CA:true, pathlen:10 # "critical" specifies that the extension is important and must be read by the platform. CA says if it is the CA certificate so it can be used to sign different certificates. "pathlen" specifies the maximum path length between this certificate and the device certificate in the chain of certificates during authentication. Path length is set here only to show how it is done. If you do not want to specify max path length, you can keep only the "basicConstraints = critical, CA:true" part here.
    keyUsage = digitalSignature, cRLSign, keyCertSign # specifies permitted key usages.

    # Default extensions for the device certificate. This tag is not used directly anywhere in this file, but will be used from the command line to create signed certificate with "-extensions v3_signed" parameter.
    [ v3_signed ]
    subjectKeyIdentifier = hash
    authorityKeyIdentifier = keyid,issuer
    basicConstraints = critical, CA:false
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment

    # default extensions for certificate revocation list
    [ crl_ext ]
    authorityKeyIdentifier=keyid:always

    # Policy of certificates signing. It specifies which certificate fields must be filled in during certificate creation. There are three possible values here:
    # "optional" - field value can be empty
    # "supplied" - field value must be filled in
    # "match" - signed certificate field value must match the CA certificate value to be created
    [ signing_policy ]
    countryName             = optional
    stateOrProvinceName     = optional
    organizationName        = optional
    organizationalUnitName  = optional
    commonName              = supplied # every certificate should have a unique common name, so this value should not be changed.
    emailAddress            = optional
    ```

7. Create a private key with aes256 encryption and a length of at least 2048 bits. You will also be asked to set the password for the key during its creation:

    `openssl genrsa -aes256 -out caKey.pem 4096`

8. Create a self-signed certificate using specifications from the configuration file. The "days" parameter says how long this certificate will be valid since the generation, so set it as you prefer:

    `openssl req -config caConfig.cnf -key caKey.pem -new -x509 -days 7300 -sha256 -extensions v3_ca -out caCert.pem`

9. You can print the created certificate with the command:

    `openssl x509 -noout -text -in caCert.pem`

#### Creating an intermediate certificate

The intermediate certificate is signed by the CA certificate, but will also be used to sign device certificates.
This step is optional.
If you are fine with signing all the device certificates with one common CA certificate, then you can skip this step.
However, if you need some certificates between the CA certificate and the device certificate then it is the way to go.
Keep in mind that in the {{< product-c8y-iot >}} cloud the maximum length of the chain of certificates is currently restricted to 2 for security reasons, so you cannot use any intermediate certificate between your CA certificate and the device certificate there.
However, you can change this behavior for dedicated installations by changing a platform wide configuration setting and increasing the allowed maximum length of the chain of certificates to more than 2.
If you change the maximum chain length for your dedicated installation, we strongly recommend you to use the proof of possession feature to protect your service from DOS attacks.
To create the intermediate certificate:

1. Create a new directory for intermediate certificates inside the caCertificate path: `mkdir intermediateCertificate`
2. Go to this directory and create a configuration file for your intermediate certificate: `touch intermediateConfig.cnf`
3. Create a database file for keeping the history of certificates signed by the intermediate certificate: `touch database.txt`
4. Create a serial file with initial serial number, which will be used to identify signed certificates: `echo 1000 > serial`
5. Create subdirectories for the signed certificates and the certificate revocation list: `mkdir deviceCertificates crl`
6. Fill in the configuration file as in the CA configuration, but remember to change the general directory ("dir") to your intermediateCertificate folder. Also don't forget to change private_key, certificate and crl names from current with "ca" prefix to "intermediate" prefix (for example: caKey.pem -> intermediateKey.pem), because files with this prefix will be generated in the next steps.
7. Generate a private key for the intermediate certificate: `openssl genrsa -aes256 -out intermediateKey.pem 4096`
8. Generate a certificate signing request: `openssl req -config intermediateConfig.cnf -new -sha256 -key intermediateKey.pem -out intermediateCsr.pem`
9. Go to the caCertificate directory and generate a signed intermediate certificate. You must use the CA configuration here, because the private key specified in the configuration file will be used to sign the certificate: `openssl ca -config caConfig.cnf -extensions v3_ca -days 3650 -notext -md sha256 -in intermediateCertificate/intermediateCsr.pem -out intermediateCertificate/intermediateCert.pem`
10. Verify if the generated certificate is correctly signed by CA: `openssl verify -CAfile caCert.pem intermediateCertificate/intermediateCert.pem`   

#### Creating a device certificate signed by CA or intermediate

1. Go to the directory of your caCertificate or intermediateCertificate depending on which one is used to sign the device certificate.
2. Generate the private key for the new certificate: `openssl genrsa -aes256 -out deviceCertificates/deviceKey.pem 4096`
3. Generate the certificate signing request (change "caConfig.cnf" to "intermediateConfig.cnf" if you are in the intermediateCertificate directory): `openssl req -config caConfig.cnf -new -sha256 -key deviceCertificates/deviceKey.pem -out deviceCertificates/deviceCsr.pem`
   Remember that the `commonName` of the device certificate, which you will be asked to provide in the console, must match the [ClientId](/device-sdk/mqtt/#MQTT-ClientId) of the device during the connection.
4. Generate the certificate signed by the CA or intermediate (change "caConfig.cnf" to "intermediateConfig.cnf" if you are in the intermediateCertificate directory): `openssl ca -config caConfig.cnf -extensions v3_signed -days 365 -notext -md sha256 -in deviceCertificates/deviceCsr.pem -out deviceCertificates/deviceCert.pem`
5. Verify if the generated certificate is correctly signed by CA or intermediate (change "caCert.pem" to "intermediateCert.pem" if you are in the intermediateCertificate directory): `openssl verify -partial_chain -CAfile caCert.pem deviceCertificates/deviceCert.pem`    

#### Creating the chain of certificates

Go into your caCertificate directory.

If you created the CA certificate, which was used to sign the intermediate certificate and then the intermediate certificate was used to sign the device certificate, then you create your chain with the command:

```shell
cat intermediateCertificate/deviceCertificates/deviceCert.pem intermediateCertificate/intermediateCert.pem caCert.pem > intermediateCertificate/deviceCertificates/deviceCertChain.pem
```
If you are not using the intermediate certificate then the command is:

```shell
cat deviceCertificates/deviceCert.pem caCert.pem > deviceCertificates/deviceCertChain.pem
```

If you are using multiple intermediate certificates between the CA certificate and the device certificate, then remember that you must keep the correct order during the chain creation (Every certificate must be followed by the certificate, which it is signed by).

#### Creating keystore and truststore

1. Go into your deviceCertificates directory with the device's private key and the generated chain of certificates. If you are using an intermediate certificate between the CA certificate and the device certificate then it will be the `caCertificate/intermediateCertificate/deviceCertificates` path, otherwise it will be `caCertificate/deviceCertificates`. Create keystore using the generated chain of certificates and the private key of the device: `openssl pkcs12 -export -name devicekeyentry -inkey deviceKey.pem -in deviceCertChain.pem -out deviceKeystore.pkcs12`
2. If you want to convert your keystore to JKS format then you would need the Java Keytool which is usually downloaded together with Java Development Kit: `keytool -importkeystore -srckeystore deviceKeystore.pkcs12 -srcstoretype PKCS12 -destkeystore deviceKeystore.jks -deststoretype JKS`
3. If you do not have the server certificate, get it by the command: `openssl s_client -showcerts -connect <cumulocity url>:<mqtt mutual ssl port (currently 8883, but that can be changed in the future)> | openssl x509 -outform PEM > serverCertificate.pem`
4. Now you can create a truststore, which will contain the server certificate. It must be created with Java Keytool (openssl does not support creating truststore, so if you don't want to use Java keytool then you must keep every trusted certificate in a separate PEM file).
   Remember that `alias` is the unique identifier for every keystore or truststore entry. It means that if you want to add a second trusted certificate to the same truststore then you must change the alias from `servercertificate` in the command below to some other name:
    * In PKCS12 format: `keytool -importcert -noprompt -keystore deviceTruststore.pkcs12 -alias servercertificate -file serverCertificate.pem`
    * In JKS format: `keytool -import -file serverCertificate.pem -alias servercertificate -keystore deviceTruststore.jks`
5. Optionally, instead of creating a new file for the truststore, you can add the trusted certificates to your created keystore and store everything in one file, which is not the recommended solution:
    * If your keystore is in the PKCS12 format: `keytool -importcert -noprompt -keystore deviceKeystore.pkcs12 -alias servercertificate -file serverCertificate.pem`
    * If your keystore is in the JKS format: `keytool -import -file serverCertificate.pem -alias servercertificate -keystore deviceKeystore.jks`
6. You can check the content of your keystore (or truststore) with the command: `keytool -list -v -keystore deviceKeystore.jks`

### How to test created certificates with MQTT.fx client

### Keystore and truststore

Generate a keystore and a truststore as described in [Generating and signing certificates](#generating-and-signing-certificates) if you didn't do it yet.

### Upload your CA certificate

Upload your CA (or intermediate) certificate to the platform. This operation will add your uploaded certificate to the server's truststore. It can be done in two ways, both of which have a role requirement of either ROLE_TENANT_ADMIN or ROLE_TENANT_MANAGEMENT_ADMIN:

**Via UI:**

1. Open the Device Management application, then navigate to the **Management** tab and select **Trusted certificates**.
2. Drop your caCert.pem (or intermediateCert.pem).
3. Check the auto-registration field.
4. Click on the certificate status to set it to **Enabled**.
5. Insert some custom name.
6. Click **Add certificate**.

After completing all the steps except adding the certificate, the form should look like this:

![Trusted certificate addition](/images/mqtt/mqttTrustedCertificateAddition.png)

Then the added certificate should be visible:

![Trusted certificate added](/images/mqtt/mqttTrustedCertificateAdded.png)

**Via REST:**

1. Display your CA (or intermediate) certificate, which you want to upload to the {{< product-c8y-iot >}} platform and copy its PEM value, which starts with "-----BEGIN CERTIFICATE-----" and ends with "-----END CERTIFICATE-----" (including the hyphens). Remove new line symbols (`\n`) if they were added automatically at the end of each line: `openssl x509 -in caCert.pem -text`
2. Send it to the platform via POST request:

```text
    POST /tenant/tenants/<TENANT_ID>/trusted-certificates
    Host: https://<TENANT_DOMAIN>/
    Authorization: Basic <YOUR_AUTHENTICATION>
    Content-Type: application/json
    {
    	"status" :  "ENABLED",
    	"name" : "certificateName",
    	"autoRegistrationEnabled" : "true",
    	"certInPemFormat" : "<CERT_PEM_VALUE>"
    }
```
### Perform a proof of possession

{{< product-c8y-iot >}} platform uses X.509 certificates to authenticate end devices.
The certificates work with a chain of trust: you can create trustworthy subcertificates with a trusted certificate.
Each certificate consists of a public and a private part.
Also see asymmetric encryption: https://en.wikipedia.org/wiki/Public-key_cryptography

{{< product-c8y-iot >}} platform receives the public part of each certificate that is to be used for device authentication.
The assignment of the device to a tenant is also done by the certificate, since each certificate must be uniquely assigned.
Performing the proof of possession steps filters out all certificates without prior proof of possession, giving preference to tenant mappings of certificates with a validated proof of possession.

However, since the public part of a certificate (and the subcertificates) is not secret, anyone on the internet theoretically has access to it.
A potential attacker could upload the public part of a certificate to {{< product-c8y-iot >}} platform even if he does not have access to the private part of the certificate (thus not being the owner of the certificate).

To ensure verification of ownership by the uploader, a proof of possession is required by the platform.

The steps for the proof of possession are as follows:
- Ensure that the certificate has been uploaded properly:
![Check certificate upload](/images/mqtt/devmgmt-mqtt-cert-pop-check.png)
 
- Download of the unsigned code:
![Download unsigned code](/images/mqtt/devmgmt-mqtt-cert-pop-downloadunsigned.png)

- Encryption of the unsigned coce by the certificate owner, which makes the key a signed key.

- Upload of the signed code to the platform:
![Upload signed code](/images/mqtt/devmgmt-mqtt-cert-pop-uploadsigned.png)

- The proof of possession is confirmed if the uploaded signed key matches the signed key expected by the platform:
![Proof of possession completed](/images/mqtt/devmgmt-mqtt-cert-pop-completed.png)


### Install and configure the MQTT client

1. Download and install the newest MQTT.fx client from: http://www.jensd.de/apps/mqttfx/
2. In MQTT.fx click **Extras** at the top and then **Edit Connection Profiles**.
3. Edit the connection profiles like so:
    - Insert the {{< company-c8y >}} URL in the **Broker address** line.
    - Insert the SSL port in the **Broker port** line.
    - In the **Client ID** field, insert the common name of your device certificate.
    - Select SSL/TLS as the authentication type.
    - Click **Enable SSL/TLS**.
    - Select **TSL v1.2** or **TSL v1.3**.
    - Select **Self signed certificates in keystores**
    - In **Keystore File** insert the path to your deviceTruststore file with either JKS or PKCS12 format.
    - In **Trusted Keystore Alias** insert **servercertificate** or a different value if you provided a different alias in step 3 above.
    - In **Trusted Keystore Password** insert the password, which you created during the deviceTruststore file creation.
    - In **Client Keystore** insert the path to your deviceKeystore file with either JKS or PKCS12 format.
    - In **Client Keystore Password** insert the password you created during the deviceKeystore creation.
    - In **Client KeyPair Alias** insert **devicekeyentry** or a different value if you provided a different alias in the "-name" parameter during the step about keystore creation in [Generating and signing certificates](#generating-and-signing-certificates).
    - In **Client KeyPair Password** insert the password, which you created during the deviceKey.pem creation.
    - The **PEM formatted** field should be checked.
4. Save and close the settings.
5. Select the edited profile and click connect.
6. You should be succesfully connected and the buttons **Disconnect**, **Publish** and **Subscribe** should be active now. This means that your connection with the certificates work correctly.

The connection settings should look like this:

![MQTT.fx configuration](/images/mqtt/mqttFxConfig.png)

### MQTT example client

The code of the {{< product-c8y-iot >}} MQTT example client implemented in Java, which connects to the platform using x.509 certificates, is available here: https://github.com/SoftwareAG/cumulocity-examples/tree/develop/mqtt-client.
This example client uses the implementation of Eclipse Paho, which is described in detail on their website: https://www.eclipse.org/paho/index.php?page=documentation.php.

Here is an example that shows how to add the needed dependency in Maven to use Eclipse Paho client:

    <dependency>
        <groupId>org.eclipse.paho</groupId>
        <artifactId>org.eclipse.paho.client.mqttv3</artifactId>
        <version>${paho.version}</version>
    </dependency>

Then the instance of the MQTT client can be created with a single line:


    MqttClient mqttClient = new MqttClient(BROKER_URL, "d:" + CLIENT_ID, new MemoryPersistence());

The BROKER_URL should contain protocol, url and port, which the client will connect to, like this: `ssl://<cumulocity url>:8883`.
The CLIENT_ID value must match the value of the common name of the device certificate that will be used.
The "d:" prefix is used in {{< product-c8y-iot >}} for device connections and it should not be removed or changed.
Now the only thing that needs to be configured to establish the SSL connection is to fill paths in the code fragment:

    sslProperties.put(SSLSocketFactoryFactory.KEYSTORE, getClass().getClassLoader().getResource(KEYSTORE_NAME).getPath());
    sslProperties.put(SSLSocketFactoryFactory.KEYSTOREPWD, KEYSTORE_PASSWORD);
    sslProperties.put(SSLSocketFactoryFactory.KEYSTORETYPE, KEYSTORE_FORMAT);
    sslProperties.put(SSLSocketFactoryFactory.TRUSTSTORE, getClass().getClassLoader().getResource(TRUSTSTORE_NAME).getPath());
    sslProperties.put(SSLSocketFactoryFactory.TRUSTSTOREPWD, TRUSTSTORE_PASSWORD);
    sslProperties.put(SSLSocketFactoryFactory.TRUSTSTORETYPE, TRUSTSTORE_FORMAT);

* KEYSTORE_NAME is the path to your keystore, which contains the private key and the chain of certificates, which the device will use to authenticate itself.
* KEYSTORE_PASSWORD is the password created for keystore to use its private key.
* KEYSTORE_FORMAT should be "JKS" or "PKCS12" depending on the file format. The path is provided by KEYSTORE_NAME.
* TRUSTSTORE_NAME is the path to your truststore, which contains the certificate of the server.
* TRUSTSTORE_PASSWORD is the password to access the truststore.
* TRUSTSTORE_FORMAT should be "JKS" or "PKCS12" depending on the file format. The path is provided by TRUSTSTORE.

After filling in this data, the example client will use the provided data to connect to the specified platform using certificates.
The example also shows how to create the callback for the connection.
First thing is to create the class which implements the interface `MqttCallbackExtended`.
Then such a class can be created and an instance of it can be provided to the MQTT client: `mqttClient.setCallback(this);`.

In general, the MQTT Eclipse Paho Client uses the Java Secure Socket Extension, which is part of the Java Development Kit, to provide secure connections via SSL.
JSSE provides the Java implementation of the SSL and TLS protocol, which can be configured by developers using its classes.
The documentation of the Java Secure Socket Extension shows how the SSL connection is established and provides some examples of customizing the implementation.
The full document is available on the [official Oracle website](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html).
