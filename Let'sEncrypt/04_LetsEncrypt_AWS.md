
# 1 Using LetsEncrypt SSL certificates with AWS Certificate Manager and CloudFront

https://gist.github.com/chrisjm/32a782317e377d52cc95fda8777e8dfe#using-letsencrypt-ssl-certificates-with-aws-certificate-manager-and-cloudfront

This is a document for managing LetsEncrypt certificates on AWS using AWS Certificate Manager and configuring on CloudFront using the AWS CLI.

---

1 Setup
Follow the instructions to set up the certbot and aws commands on your local machine:

    certbot setup
    AWS CLI setup



2 LetsEncrypt
Obtaining the Certificate via certbot
    certbot certonly --manual

Follow the instructions. If all goes well, your certificate will be in `/etc/letsencrypt/live/<fqdn>,` where `<fqdn>` is the fully-qualified domain name  ( eg. www.example.com, example.com, etc. ) 


3 Amazon Web Services
CloudFront

Import the certificate into IAM:
    aws iam upload-server-certificate --server-certificate-name alphaPWServerCertificate --certificate-body file://etc/letsencrypt/live/



