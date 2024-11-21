
# 1 COMPONENTS


![](image/Pasted%20image%2020241120224015.png)


**Certificate Authority (CA)**
Trusted entity responsible for issuing and managing digital certificates. It verifies digital identities before issuing certificates and signs them to affirm their validity
获取用户的私人信息和用户的公钥,  用ca自己的私钥 将 用户的私人信息和用户的公钥 加密为 用户自己的Digital Certifi cates.   
用户A 从 ca 处 获得 用户a自己的数字证书 和 ca自己的公钥 
用户A将自己的数字证书 给用户b,  用户b 用 ca的公钥 解密 用户A的数字证书


**Registration Authority (RA)**
Acts as an intermediary between service provider and the CA. It helps verify identities before passing certificate signing requests to the CA


**Digital Certificates**
Electronic credentials that bind a public key to the identity of its owner


**Public and Private Keys**
Used to encrypt/decrypt data or to create/verify digital signatures Certificate Repository: Secure storage location where issued certificates and their status are stored


**Certificate Revocation List**
List of digital certificates that have been revoked before their delivery date
证明那些 digital certificate 还有效

**End Entities**
Users, devices, and systems that use digital certificates to secure their communication

# 2 CERTIFICATES?

- X.509 certificates are based on a widely used standard for digital certificates to authenticate the identity of entities and secure communication in various networking environments
- They are integral part of the PKI and enable secure data exchange by binding a public key to an entity
- Part of the ITU-T X.500 series of recommendations developed by the International Telecommunication Union (ITU-T) for directory services
- X.500 is a framework for directory services for the definition of protocols and data formats used for managing distributed directories of information, such as public key information
- X.509 was introduced in 1988 to define the standard format for public key certificates used in PKIs

USE CASES
- ==TLS/SSL: When using HTTPS, X.509 certificates are used to authenticate the server and to establish an encrypted connection between server and user==
- Email Encryption: Secure/Multipurpose Internet Mail Extensions (S/MIME) uses X.509 certificates to encrypt email content and verify sender identity
- VPN and Network Authentication: X.509 certificates are used to authenticate devices connecting to a Virtual Private Network (VPN)


# 3 HOW TO GET A CERTIFICATE FROM A SERVER WITH OPENSSL


![](image/Pasted%20image%2020241120225132.png)

```
echo | openssl s_client -connect tu.berlin:443 -servername tu.berlin | \sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > tu-berlin-cert.pem


echo | openssl s_client -connect 141.23.73.70:443 -servername tu.berlin | \sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > tu-berlin-cert.pem

openssl x509 -in tu-berlin-cert.pem -text -noout > tu-berlin-cert-details.txt

```



# 4 解释证书中的各个值


```
-----BEGIN CERTIFICATE-----
MIIHWjCCBUKgAwIBAgIRAKobBeVfI8dlqYyewNrR5b8wDQYJKoZIhvcNAQEMBQAw
RDELMAkGA1UEBhMCTkwxGTAXBgNVBAoTEEdFQU5UIFZlcmVuaWdpbmcxGjAYBgNV
BAMTEUdFQU5UIE9WIFJTQSBDQSA0MB4XDTI0MDgyOTAwMDAwMFoXDTI1MDgyOTIz
NTk1OVowXzELMAkGA1UEBhMCREUxDzANBgNVBAgTBkJlcmxpbjEnMCUGA1UECgwe
VGVjaG5pc2NoZSBVbml2ZXJzaXTDpHQgQmVybGluMRYwFAYDVQQDEw13d3cudHUu
YmVybGluMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA498+oY9/MTf3
CcxO01Hx0a8ccXERtzVfvt0JqarCWVYuk+d+Ccfnz/Pq8SF/pZQU693mtpKWWHRT
VQ4PHHxGfRIY8LBWLGjbYaeF58gTdNC73lJT9pliaqMgF2GEEO0/716XAC1HSlqb
PxXPZ7r73KeHVB/v9taGcWuYa2JSbZ60Dpi29PUwI7tsAcvXF6AjhUfjvioxBWOw
GqyUFM0f7UovQJ9GXQbz22ic9d3+AK+NfC8EwJgY8eKPN9fV5xXjNL7ErdGYZEil
4c0mg27M84BzSaBULzxctwNqAf8TaRQCwhzgr50/7bIjLPj1EvJ2aFaJBewz4lFg
ypoeR6AQDwIDAQABo4IDKjCCAyYwHwYDVR0jBBgwFoAUbx01SRBsMvpZoJ68iugf
lb5xegwwHQYDVR0OBBYEFMHfcbW/HgvJxJM3+CtySsNyHGrdMA4GA1UdDwEB/wQE
AwIFoDAMBgNVHRMBAf8EAjAAMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcD
AjBJBgNVHSAEQjBAMDQGCysGAQQBsjEBAgJPMCUwIwYIKwYBBQUHAgEWF2h0dHBz
Oi8vc2VjdGlnby5jb20vQ1BTMAgGBmeBDAECAjA/BgNVHR8EODA2MDSgMqAwhi5o
dHRwOi8vR0VBTlQuY3JsLnNlY3RpZ28uY29tL0dFQU5UT1ZSU0FDQTQuY3JsMHUG
CCsGAQUFBwEBBGkwZzA6BggrBgEFBQcwAoYuaHR0cDovL0dFQU5ULmNydC5zZWN0
aWdvLmNvbS9HRUFOVE9WUlNBQ0E0LmNydDApBggrBgEFBQcwAYYdaHR0cDovL0dF
QU5ULm9jc3Auc2VjdGlnby5jb20wggF9BgorBgEEAdZ5AgQCBIIBbQSCAWkBZwB1
AN3cyjSV1+EWBeeVMvrHn/g9HFDf2wA6FBJ2Ciysu8gqAAABkZ4TOZMAAAQDAEYw
RAIgZ9pyUvF1jUDJzs8TdP6gZIDXVwVRMbEc/3o/1Avpb/MCIBCnNslEZ6j4QAMK
5lb9OCnltOUopHn2R1Y4so77kEVwAHYADeHyMCvTDcFAYhIJ6lUu/Ed0fLHX6TDv
DkIetH5OqjQAAAGRnhM5JAAABAMARzBFAiEAl2vZnihOV0mjdUOQtGPv7WXL4+jE
m4g9ZshmOZxVGvcCIAi4P8hE1sXKNa21RjMCXZ526pFkCdnTS3CnGt00q3VGAHYA
EvFONL1TckyEBhnDjz96E/jntWKHiJxtMAWE6+WGJjoAAAGRnhM5LQAABAMARzBF
AiEA4+HvoZW2mU/wwaH5j2RZaIPqfNIgBEwbVCMS+GVNrQYCIGvplyjYpLXGJsgz
KZKlbgq3YAzQY71IIXuTnyMnhMItMCMGA1UdEQQcMBqCDXd3dy50dS5iZXJsaW6C
CXR1LmJlcmxpbjANBgkqhkiG9w0BAQwFAAOCAgEACpnUSJ8c4QAw+yO9VPAoGJ84
DeXLxF6TygRDeBDaTEzmyhwgp60dUvQFl5eDZ39x+VPUKz6ve85QvpEJ3J5EPj6g
ttnEF0iZ+mIUmdBWNCfxuaWYxQvHVTXTpZkuU+V14jVIv7kJY57wzaq+kA5GY6BR
l0pcgyV2Hr5a7kI/bLRlOHnBp553iZmagt4MpZX0kylUqaBre5eJ5A5MCNWZ163p
TvVzIoUKACki8w0u+Au360y9U8iDCWYbq5exqBjD5H7va4+qcCXpduaxn8S1SgMq
E+dPHukLAl6ik7uOgpjuCaajceKjFDBPexhWoZmx1zKqvbg0WG3hJrR8Bio9Lt58
2FqKVWAaUSO0YcdqynlwpsW6m94/wBduMacfVL28hBwdSElqzvpqeqj3EmTwqgGv
7qbVmeXzvFAYeUzKLxvq3Zx9kdCA14jmqTAM1sm/+zGM1u0SNb9yD/sFjGbrhRLn
TGvUDKz7frIA/O0QUtWyv14lm0AyGNKGG+uBpid5g6wliMukdEJZWwttLoiG1/Pd
0Q9cAY4hMH0JIWaUpdWgMDDzhPAJuoqOYm3E3rDbRjS0KtugmpdT0Yj/a6Bs4VNU
b+5eUfx/q9aP1fgT5LAI8N5+KrzzxzSShk8sooh+42QUTrR5DPq1ZeQmHMoOth29
lf31QDFRM3fIlpS2aho=
-----END CERTIFICATE-----

```


```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            aa:1b:05:e5:5f:23:c7:65:a9:8c:9e:c0:da:d1:e5:bf
        Signature Algorithm: sha384WithRSAEncryption
        Issuer: C = NL, O = GEANT Vereniging, CN = GEANT OV RSA CA 4
        Validity
            Not Before: Aug 29 00:00:00 2024 GMT
            Not After : Aug 29 23:59:59 2025 GMT
        Subject: C = DE, ST = Berlin, O = Technische Universit\C3\A4t Berlin, CN = www.tu.berlin
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:e3:df:3e:a1:8f:7f:31:37:f7:09:cc:4e:d3:51:
                    f1:d1:af:1c:71:71:11:b7:35:5f:be:dd:09:a9:aa:
                    c2:59:56:2e:93:e7:7e:09:c7:e7:cf:f3:ea:f1:21:
                    7f:a5:94:14:eb:dd:e6:b6:92:96:58:74:53:55:0e:
                    0f:1c:7c:46:7d:12:18:f0:b0:56:2c:68:db:61:a7:
                    85:e7:c8:13:74:d0:bb:de:52:53:f6:99:62:6a:a3:
                    20:17:61:84:10:ed:3f:ef:5e:97:00:2d:47:4a:5a:
                    9b:3f:15:cf:67:ba:fb:dc:a7:87:54:1f:ef:f6:d6:
                    86:71:6b:98:6b:62:52:6d:9e:b4:0e:98:b6:f4:f5:
                    30:23:bb:6c:01:cb:d7:17:a0:23:85:47:e3:be:2a:
                    31:05:63:b0:1a:ac:94:14:cd:1f:ed:4a:2f:40:9f:
                    46:5d:06:f3:db:68:9c:f5:dd:fe:00:af:8d:7c:2f:
                    04:c0:98:18:f1:e2:8f:37:d7:d5:e7:15:e3:34:be:
                    c4:ad:d1:98:64:48:a5:e1:cd:26:83:6e:cc:f3:80:
                    73:49:a0:54:2f:3c:5c:b7:03:6a:01:ff:13:69:14:
                    02:c2:1c:e0:af:9d:3f:ed:b2:23:2c:f8:f5:12:f2:
                    76:68:56:89:05:ec:33:e2:51:60:ca:9a:1e:47:a0:
                    10:0f
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Authority Key Identifier: 
                6F:1D:35:49:10:6C:32:FA:59:A0:9E:BC:8A:E8:1F:95:BE:71:7A:0C
            X509v3 Subject Key Identifier: 
                C1:DF:71:B5:BF:1E:0B:C9:C4:93:37:F8:2B:72:4A:C3:72:1C:6A:DD
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Basic Constraints: critical
                CA:FALSE
            X509v3 Extended Key Usage: 
                TLS Web Server Authentication, TLS Web Client Authentication
            X509v3 Certificate Policies: 
                Policy: 1.3.6.1.4.1.6449.1.2.2.79
                  CPS: https://sectigo.com/CPS
                Policy: 2.23.140.1.2.2
            X509v3 CRL Distribution Points: 
                Full Name:
                  URI:http://GEANT.crl.sectigo.com/GEANTOVRSACA4.crl
            Authority Information Access: 
                CA Issuers - URI:http://GEANT.crt.sectigo.com/GEANTOVRSACA4.crt
                OCSP - URI:http://GEANT.ocsp.sectigo.com
            CT Precertificate SCTs: 
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : DD:DC:CA:34:95:D7:E1:16:05:E7:95:32:FA:C7:9F:F8:
                                3D:1C:50:DF:DB:00:3A:14:12:76:0A:2C:AC:BB:C8:2A
                    Timestamp : Aug 29 12:19:05.747 2024 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:44:02:20:67:DA:72:52:F1:75:8D:40:C9:CE:CF:13:
                                74:FE:A0:64:80:D7:57:05:51:31:B1:1C:FF:7A:3F:D4:
                                0B:E9:6F:F3:02:20:10:A7:36:C9:44:67:A8:F8:40:03:
                                0A:E6:56:FD:38:29:E5:B4:E5:28:A4:79:F6:47:56:38:
                                B2:8E:FB:90:45:70
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : 0D:E1:F2:30:2B:D3:0D:C1:40:62:12:09:EA:55:2E:FC:
                                47:74:7C:B1:D7:E9:30:EF:0E:42:1E:B4:7E:4E:AA:34
                    Timestamp : Aug 29 12:19:05.636 2024 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:21:00:97:6B:D9:9E:28:4E:57:49:A3:75:43:
                                90:B4:63:EF:ED:65:CB:E3:E8:C4:9B:88:3D:66:C8:66:
                                39:9C:55:1A:F7:02:20:08:B8:3F:C8:44:D6:C5:CA:35:
                                AD:B5:46:33:02:5D:9E:76:EA:91:64:09:D9:D3:4B:70:
                                A7:1A:DD:34:AB:75:46
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : 12:F1:4E:34:BD:53:72:4C:84:06:19:C3:8F:3F:7A:13:
                                F8:E7:B5:62:87:88:9C:6D:30:05:84:EB:E5:86:26:3A
                    Timestamp : Aug 29 12:19:05.645 2024 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:21:00:E3:E1:EF:A1:95:B6:99:4F:F0:C1:A1:
                                F9:8F:64:59:68:83:EA:7C:D2:20:04:4C:1B:54:23:12:
                                F8:65:4D:AD:06:02:20:6B:E9:97:28:D8:A4:B5:C6:26:
                                C8:33:29:92:A5:6E:0A:B7:60:0C:D0:63:BD:48:21:7B:
                                93:9F:23:27:84:C2:2D
            X509v3 Subject Alternative Name: 
                DNS:www.tu.berlin, DNS:tu.berlin
    Signature Algorithm: sha384WithRSAEncryption
    Signature Value:
        0a:99:d4:48:9f:1c:e1:00:30:fb:23:bd:54:f0:28:18:9f:38:
        0d:e5:cb:c4:5e:93:ca:04:43:78:10:da:4c:4c:e6:ca:1c:20:
        a7:ad:1d:52:f4:05:97:97:83:67:7f:71:f9:53:d4:2b:3e:af:
        7b:ce:50:be:91:09:dc:9e:44:3e:3e:a0:b6:d9:c4:17:48:99:
        fa:62:14:99:d0:56:34:27:f1:b9:a5:98:c5:0b:c7:55:35:d3:
        a5:99:2e:53:e5:75:e2:35:48:bf:b9:09:63:9e:f0:cd:aa:be:
        90:0e:46:63:a0:51:97:4a:5c:83:25:76:1e:be:5a:ee:42:3f:
        6c:b4:65:38:79:c1:a7:9e:77:89:99:9a:82:de:0c:a5:95:f4:
        93:29:54:a9:a0:6b:7b:97:89:e4:0e:4c:08:d5:99:d7:ad:e9:
        4e:f5:73:22:85:0a:00:29:22:f3:0d:2e:f8:0b:b7:eb:4c:bd:
        53:c8:83:09:66:1b:ab:97:b1:a8:18:c3:e4:7e:ef:6b:8f:aa:
        70:25:e9:76:e6:b1:9f:c4:b5:4a:03:2a:13:e7:4f:1e:e9:0b:
        02:5e:a2:93:bb:8e:82:98:ee:09:a6:a3:71:e2:a3:14:30:4f:
        7b:18:56:a1:99:b1:d7:32:aa:bd:b8:34:58:6d:e1:26:b4:7c:
        06:2a:3d:2e:de:7c:d8:5a:8a:55:60:1a:51:23:b4:61:c7:6a:
        ca:79:70:a6:c5:ba:9b:de:3f:c0:17:6e:31:a7:1f:54:bd:bc:
        84:1c:1d:48:49:6a:ce:fa:6a:7a:a8:f7:12:64:f0:aa:01:af:
        ee:a6:d5:99:e5:f3:bc:50:18:79:4c:ca:2f:1b:ea:dd:9c:7d:
        91:d0:80:d7:88:e6:a9:30:0c:d6:c9:bf:fb:31:8c:d6:ed:12:
        35:bf:72:0f:fb:05:8c:66:eb:85:12:e7:4c:6b:d4:0c:ac:fb:
        7e:b2:00:fc:ed:10:52:d5:b2:bf:5e:25:9b:40:32:18:d2:86:
        1b:eb:81:a6:27:79:83:ac:25:88:cb:a4:74:42:59:5b:0b:6d:
        2e:88:86:d7:f3:dd:d1:0f:5c:01:8e:21:30:7d:09:21:66:94:
        a5:d5:a0:30:30:f3:84:f0:09:ba:8a:8e:62:6d:c4:de:b0:db:
        46:34:b4:2a:db:a0:9a:97:53:d1:88:ff:6b:a0:6c:e1:53:54:
        6f:ee:5e:51:fc:7f:ab:d6:8f:d5:f8:13:e4:b0:08:f0:de:7e:
        2a:bc:f3:c7:34:92:86:4f:2c:a2:88:7e:e3:64:14:4e:b4:79:
        0c:fa:b5:65:e4:26:1c:ca:0e:b6:1d:bd:95:fd:f5:40:31:51:
        33:77:c8:96:94:b6:6a:1a

```


![](image/Pasted%20image%2020241121093733.png)

![](image/Pasted%20image%2020241121093746.png)

![](image/Pasted%20image%2020241121093755.png)

## 4.1 一些基础信息 

![](image/Pasted%20image%2020241121095539.png)

- Version 
    - version 3 is the latest and most popular version of the X.509 certifi cation format
- Serial Number
    - contains the serial number of the certificate, defined by the certifi cation authority, and used for certificate revocation
- Issuer
    - defines certification authority with their common name and country
    - `Issuer: C = NL, O = GEANT Vereniging, CN = GEANT OV RSA CA 4`
        - c: country, st: State , o: organization, 
        - cn: common name , Common Name field historically carries the subject's primary identifiers, but modern standards and software strongly recommend using the Subject Alternative Name (SAN) extension.  就是 这个ca (www.tu.berlin) 还被 其他的 ca (GEANT OV RSA CA 4) 再 identified . 
- Validity 
    - specifies the validity period of the certificate
- Subject 
    - information about the certificate owner (subject), including country, region, and common name (domain name, personal name, IP address, email address,...)
    - certification authority根据 certificate owner的个人信息和certificate owner的 public key,  生成这个certificate owner的电子证书, 给certificate owner. 
    - `Subject: C = DE, ST = Berlin, O = Technische Universit\C3\A4t Berlin, CN = www.tu.berlin`
        - c: country, st: State , o: organization, cn: common name , 就是 domain name
- Subject Public Key Info
    - public key of the subject, including public key algorithm and key length. 
    - certificate owner 他的 public key 的信息 
- Note
    - Common Name field historically carries the subject's primary identifiers, but modern standards and software strongly recommend using the Subject Alternative Name (SAN) extension


## 4.2 x509v3 extensions

![](image/Pasted%20image%2020241121095604.png)



- ==Extensions allow a certificate to carry extra metadata that helps define how it should be used and what its capabilities and restrictions are==
- Two purposes:
    - Adding additional attributes such as policy, subject alternative names, etc. 
    - Conveying restrictions on how a certificate should be used or trusted
- Each extension consists of three components:
    - Object Identifier (OID) identify a specific extension type in a certificate
    - If an extension is marked critical, the application must understand and process the extension. If the application does not understand it must reject the certificate as invalid
    - If an extension is market non-critical it is optional and may be ignored if the application does not recognize it
    - The extension value carries the actual data related to the extension
- X.509 extensions are largely standardized by the ITU-T (International Telecommunication Union) and the IETF (Internet Engineering Task Force), but also customized extension are allowed (e.g. for defining company-specific identifiers or security policies)




- Key Usage
    - Defines the permitted uses of the key (e.g., digital signature, key encipherment)
    - `X509v3 Key Usage: critical , Digital Signature, Key Encipherment`   
    - 说明了这个key用途是 Digital Signature, Key Encipherment
- Extended Key Usage 
    - Specifiers additional high-level purposes (e.g., server authentication, client authentication)
    - ` X509v3 Extended Key Usage: TLS Web Server Authentication, TLS Web Client Authentication`
- Basic Constraints
    - Indicates if the certificate belongs to a Certificate Authority
- Subject Alternative Name 
    - Allows specifying multiple domain names, IP addresses, or email addresses
- Authority Key Identifier 
    - Identifies the issuing CA's key that signed the certificate
- CRL Distribution Point
    - Specifies locations for retrieving the Certificate Revocation List (CRL)
- Certificate Policies
    - Lists the policies under which the certificate was issued
- Authority Information Access
    - includes the URL of the CA where issued certificate can be retrieved and the URL of the Online Certificate Status Protocol (OCSP) responder
### 4.2.1 CT Precertificate SCTSs

![](image/Pasted%20image%2020241121101524.png)


CT Precertificate SCTSs
- Certificate Transparency (CT) helps to monitor and audit SSL/TLS certificates and requires that all issued certificates are logged in publicly accessible, tamperproof certificate logs
- Helps to detect and prevent the unauthorized issuance of certificates
- A Signed Certificate Timestamp (SCT) is a proof that the CA has submitted the certificate to several event logs
- Modern browsers refuse to accept a certificate if it does not contain timestamps as an evidence of the submissions of the certifi cate to the logs
- Detailed description later in this chapter



### 4.2.2 key usages
- Key Usage
    - Defines the permitted uses of the key (e.g., digital signature, key encipherment)
    - `X509v3 Key Usage: critical , Digital Signature, Key Encipherment`   
    - 说明了这个key用途是 Digital Signature, Key Encipherment

The Key Usage Extension defines the purpose of the key contained in the certificate
The usage restriction might be employed when a key that could be used for more than one operation is to be restricted
Conforming CAs must include this extension in certificates that are used to validate digital signatures on other public key certificates or CRLs and mark this extension as critical


![](image/Pasted%20image%2020241121102344.png)

### 4.2.3 Extended Key Usage Extension

The Extended Key Usage Extension indicates one or more purposes for which the certified public key may be used, in addition to or in place of the basic purposes indicated in the key key usage extension

If a certificate contains both a key usage extension and an extended key usage extension, the both extensions must be processed independently and the certifi cate must only be used for a purpose consistent with both extensions


![](image/Pasted%20image%2020241121102615.png)



# 5 DOMAIN, ORGANIZATION, AND EXTENDED VERIFICATION



![](image/Pasted%20image%2020241121103247.png)

## 5.1 DOMAIN VALIDATION CERTIFICATES

![](image/Pasted%20image%2020241121103843.png)
没有 Organisation 项

Overview
Most basic level of SSL/TLS encryption and verifi cation
Verify that the certifi cate owner owns the domain but don't conduct further identity checks
Popular for small websites an blogs

Security Level
Do not validate the organization or the individual behind the web site
Display only the padlock symbol and the certifi cate does not contain the Organization fi eld


Verification Process
CA verifies that the applicant has control over the domain, which is achieved by...
... sending a confi rmation email to the domain's administrative contact
... adding a TXT record with a provided verifi cation string to the domain's DNS record
... placing a fi le with a verifi cation string under a predefi ned path in the domain's web root

## 5.2 ORGANIZATION VALIDATION CERTIFICATES

![](image/Pasted%20image%2020241121104037.png)


Overview
Higher level of verification, as they confirm both domain control and basic details about the organization behind the website
Suitable for e-commerce sites, business websites, and organization portals


Security Level
Certificates provide encryption and basic organization validation
User can view the organization's name in the certificate details


Verification Process
CA checks...
... domain control, similar to DV certificates
... details about the organization, such as name, location, and legal status

Verifi cation includes manual vetting by the CA, such as verifying business records and calling the organization's listed contact

## 5.3 EXTENDED VALIDATION CERTIFICATES

![](image/Pasted%20image%2020241121104303.png)


Overview
Highest level or trust and verification
Require a comprehensive validation process to provide maximum assurance about the organization's identity
EV certificates are visually distinguishable, as browsers often display the organization's name in green next to the URL

Security Level
Highest level of identity assurance
Suitable for financial institutions and highly sensitive websites


Verification Process
CA performs extensive checks, including...
- ... domain control verifi cation
- ... details about the organization, such as name, location, and legal status
- ... confirmation through legal documentation and government databases

For the vetting process, the CA must adhere to EV guidelines established by the CA/Browser Forum

# 6 BASELINE REQUIREMENTS FOR TLS SERVER CERTIFICATES

The Baseline Requirements for the Issuance and Management of Publicly-Trusted TLS Server Certifi cates are a set of standards established by the CA/Browser Forum

The CA/Browser Forum is an industry consortium operated collaboratively by CAs and web browser vendors

The forum operates through working groups, regular meetings, and a voting process among its members

Participation in the forum and adherence to its standards are mandatory, but browser and operating system vendors enforce compliance with the standards as a condition for CAs to be trusted in their ecosystems

The Baseline Requirements document defi nes the rules a CA has to follow:
- Validation of Domain Ownership: Verifi cation of the applicant's control over the domain for which the certificate is requested
- Identity Verifi cation for OV Certificates: For OV and EV certificates, CAs must perform rigorous checks on the organization's identity
- Cryptographic Standards and Certificate Fields: Specifies key sizes, algorithms, and hashes as well as optional fields in certificates
- Certificate Revocation: Requirements for how and when certificates should be revoked
- Validity Periods: Limits the maximum duration of certificates
- Subscriber Obligations: Outlines responsibilities for the certificate holder
- Certificate Transparency: Recommends to log certificates in CT Logs




# 7 THE LIFE CYCLE OF A DIGITAL CERTIFICATE

![](image/Pasted%20image%2020241121105231.png)


# 8 generate a digital certificate 

## 8.1 GENERATING A CERTIFICATE SIGNING REQUEST WITH OPENSSL


A _certificate signing request_ (_CSR_) is one of the first steps towards getting your own SSL/TLS certificate.

In [public key infrastructure](https://en.wikipedia.org/wiki/Public_key_infrastructure "Public key infrastructure") (PKI) systems, a **certificate signing request** (**CSR** or **certification request**) is a message sent from an applicant to a [certificate authority](https://en.wikipedia.org/wiki/Certificate_authority "Certificate authority") of the public key infrastructure (PKI) in order to apply for a [digital identity certificate](https://en.wikipedia.org/wiki/Public_key_certificate "Public key certificate"). The CSR usually contains the public key for which the certificate should be issued, identifying information (such as a domain name) and a proof of authenticity including integrity protection (e.g., a digital signature). T

![](image/Pasted%20image%2020241121105714.png)


![](image/Pasted%20image%2020241121105405.png)

## 8.2 APPLYING FOR A DIGITAL CERTIFICATE

![](image/Pasted%20image%2020241121105744.png)


![](image/Pasted%20image%2020241121105751.png)



## 8.3 CERTIFICATE AUTOMATION WITH ACME


![](image/Pasted%20image%2020241121105905.png)

ACME (Automated Certifi cate Management Environment) is a set of protocols designed to automate the processes of certifi cate issuance, renewal, and revocation
Popularized by the free CA Let's Encrypt, but meanwhile widely adopted by other CAs and applications as well

ADVANTAGES OF ACME
- Automation: 
    - Minimizes human intervention, reducing errors and administrative overhead
- Security: 
    - Automated certifi cate renewal helps to maintain their integrity and validity and reduced the likelihood of expired certifi cates causing security issues
- Scalability: 
    - Simplifies large-scale deployments of SSL/TLS certificates and makes it easy to secure large numbers of servers and domains
- Standardization: 
    - ACME is a standardized protocol defined in IETF RFC 8555, making it interoperable across different clients and CAs


HOW ACME WORKS (I/II)
- Account Registration: The ACME client registers an account with the target CA
- Certificate Request: The ACME client requests a certificate from the CA by sending the desired domains names(s) or IP address(es)
- Challenge and Validation: To prove that the client controls the domain(s), the CA issues a challenge, which the domain owner must complete to verify ownership
    - HTTP-01: The client must establish a specific fi le on a certain URL path via HTTP on port 80 to prove control of the domain
    - DNS-01: The client must add a DNS TXT record with a specific value to the domain's DNS zone
    - TLS-ALPN-01: The client must serve a temporary self-signed certificate with specific properties over TLS on port 443 The CA verifi es that the challenge has been tackled and confi rms the client's control over the domain
- The CA verifies that the challenge has been tackled and confirms the client's control over the domain


Certificate Issuance: Once the challenge is validated, the CA issues the certificate, and the client download it
Renewal: The ACME client regularly renews certificates before they expire without human intervention
Revocation: If a certificate needs to be revoked, the ACME client can communicate this to the CA



# 9 CERTIFICATE CHAINS


![](image/Pasted%20image%2020241121110507.png)



![](image/Pasted%20image%2020241121110528.png)

## 9.1 root CAs

![](image/Pasted%20image%2020241121110801.png)


CA3 生成 tu.berlin 的 电子签名 
tu.berlin 的 电子签名  包含了 tu.berlin 的 publickey, tu.berlin的 信息 和 signature which is issued by CA3 
Signature which is issued by CA3  是 CA3用自己的 private key  生成的.  CA3将这个 digital signture 封印在 tu.berlin 的 电子签名  中 
用 CA3 的public key  可以去解密  tu.berlin 的电子签名  

---

ROOT STORES

![](image/Pasted%20image%2020241121112500.png)


- The Trusted Root Certificate Store or Trusted Root Store or Root Store is the place in an operating system or browser that stores root certificates
- Contains pre-installed or manually added root certificates, establishing trust for certificates issued by those root CAs
- The store is managed by the operating system (such as Windows, macOS, or Linux) or by individual applications, like web browsers (Firefox)
- ==When a certificate is presented during an SSL/TLS handshake, the system checks the certificate chain against the Trusted Root Store to determine if it can trust the certificate==
- If a root certificate is not found in the Trusted Root Store, the system will alert the user with a warning that the certificate is untrusted


# 10 CERTIFICATE REVOCATION

revocation 就是 使得一个  issued digital certificate 失效 

Certificate revocation is the process of invalidating an issued digital certificate before its scheduled expiration date
Once a certificate is revoked, it is no longer trusted by browsers, operating systems, and other applications


REASONS FOR CERTIFICATE REVOCATION
- Compromised Private Keys: The private key associated with a certifi cate is compromised or stolen
- Certifi cate Misuse: For example, a certifi cate was mistakenly issued to the wrong entity
- Change in Affi liation: A certifi cate holder's affi liation changes
- CA Compromising: A CA has been compromised
- Cessation of Service: The certificate holder has ceased operations and no longer needs a certificate
- Compliance and Policy Requirements: Organizations may have security policies to revoke certificates in certain situations to comply with regulations or industry standards








