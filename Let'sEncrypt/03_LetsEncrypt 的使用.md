


# 1 

https://itnext.io/using-letsencrypt-ssl-certificates-in-aws-certificate-manager-c2bc3c6ae10

## 1.1 Install the LetsEncrypt CLI

On MacOS, the _brew_ installation route was the easy choice for me which installed LetsEncrypt’s _certbot_ CLI tool (see the LetsEncrypt documentation for installation onto other OS’s).

brew install letsencrypt

## 1.2 Generating the certificate

Once LetsEncrypt is installed, generating the SSL certificate is just a matter of running the _certbot_ CLI tool and having it verify you are the owner of the domain specified. For my usage I decided to create a wildcard certificate, covering any subdomains of my domain, indicated by the _*.arronharden.com_ option to the CLI. This meant that the same certificate could be used for any subdomain under my root domain.

By default the _certbot_ tool needs to run as root, e.g. requiring _sudo_ on MacOS. Alternatively, it can be run without sudo with the appropriate flags set to alter default directories, which is what I shall do here.

$ mkdir ~/letsencrypt  
$ cd ~/letsencrypt  
$ certbot certonly \  
    --manual \  
    --preferred-challenges=dns \  
    --email your@email-address.com \  
    --agree-tos \  
    --config-dir ./config \  
    --logs-dir ./logs \  
    --work-dir ./workdir \  
    -d *.arronharden.com



Wildcard Certificate 

A SSL/TLS Wildcard certificate is a single certificate with a wildcard character `(*) `in the domain name field. This allows the certificate to secure multiple sub domain names (hosts) pertaining to the same base domain. For example, a wildcard certificate for `*. (domainname).com`, could be used for www.



# 2 案例1


https://medium.com/jungletronics/aws-letsencrypt-bfce27decd52

Step —Run only these three lines on your [remote Ubuntu 20](https://medium.com/jungletronics/aws-essentials-40464cd094da):

```sh

#-----------------------------------------------------------$ 

**sudo apt install certbot python3-certbot-apache**$ 
**sudo certbot --apache4**#Checks whether SSL renewal is taking place successfully;$ 
**sudo certbot renew --dry-run4**

#-----------------------------------------------------------

To remove:$ sudo find /etc/letsencrypt/ -name "*[www.j3.blog.br*](http://www.j3.blog.br*)"  
$ sudo certbot delete --cert-name [MyDomain](https://medium.com/jungletronics/www.j3.blog.br)  
$ sudo certbot delete --cert-name [www.j3.blog.br](http://www.j3.blog.br*)Or remove manually:$ sudo rm -rf /etc/letsencrypt/live/${DOMAIN} rm -rf   

$ sudo /etc/letsencrypt/renewal/${DOMAIN}.conf rm -rf   
$ sudo /etc/letsencrypt/archive/${DOMAIN}#---------------------------------------------------------


```


- The first line is to install the Certbot Python package software on your Apache server via PuTTY (apt install certbot python3-certbot-apache);
- The second line (sudo certbot — apache) just get Certbot implementation for Apache Server.
- The last and final line (sudo certbot renew — dry-run) checks whether SSL renewal is taking place successfully, which means we authorize Certbot LetEncrypt to automatically renew my certificate — something that happens every three months if I’m not mistaken:)
