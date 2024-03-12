
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


证书的位置： 
antonputra.com 为 domain name 

![](image/Pasted%20image%2020240311152232.png)


# 2 案例
## 2.1 案例1


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


## 2.2 案例2 Secure Nginx with Lets Encrypt on Ubuntu 20.04 with Certbot 

### 2.2.1 安装 nginx 

add repo of nignx in ubuntu sercver
![](image/Pasted%20image%2020240311235443.png)
![](image/Pasted%20image%2020240311235455.png)


add public key for nginx repo 

![](image/Pasted%20image%2020240311235553.png)

![](image/Pasted%20image%2020240311235640.png)


安装 niginx 

![](image/Pasted%20image%2020240311235651.png)

![](image/Pasted%20image%2020240311150047.png)

---

### 2.2.2 看 nginx 里面的配置 

nginx.conf 
![](image/Pasted%20image%2020240311235733.png)

![](image/Pasted%20image%2020240311235755.png)

![](image/Pasted%20image%2020240312001112.png)



default.conf

![](image/Pasted%20image%2020240312000150.png)

![](image/Pasted%20image%2020240312000759.png)




### 2.2.3 site-available,  sites-enabled 

![](image/Pasted%20image%2020240312000807.png)
![](image/Pasted%20image%2020240312000818.png)

![](image/Pasted%20image%2020240312000830.png)
![](image/Pasted%20image%2020240312000848.png)

![](image/Pasted%20image%2020240312000903.png)




![](image/Pasted%20image%2020240312000952.png)


create a soft link form site-available to sites-enabled 


![](image/Pasted%20image%2020240312001135.png)




![](image/Pasted%20image%2020240311150133.png)

![](image/Pasted%20image%2020240311150202.png)



verify the nginx configuration 
![](image/Pasted%20image%2020240311150330.png)
![](image/Pasted%20image%2020240312001149.png)


---

### 2.2.4 安装 certbot

![](image/Pasted%20image%2020240312000432.png)

![](image/Pasted%20image%2020240312000448.png)

![](image/Pasted%20image%2020240312000504.png)


![](image/Pasted%20image%2020240312000516.png)



---

### 2.2.5 用 cerbot 来 delegate a domain 

![](image/Pasted%20image%2020240312001217.png)

![](image/Pasted%20image%2020240312000554.png)

可以看到 一个 staging certificate 被产生了 

![](image/Pasted%20image%2020240312001426.png)

--- 


![](image/Pasted%20image%2020240311150600.png)

--nginx ist the plugin to add functionallity to be able to locate that serial name block in nignx and also modify  the nignx configuration 

![](image/Pasted%20image%2020240311150725.png)


![](image/Pasted%20image%2020240312001524.png)

---


### 2.2.6 再次查看 nginx 的配置 

![](image/Pasted%20image%2020240311150824.png)

可以看出来 新增了很多项

![](image/Pasted%20image%2020240311150837.png)




---


### 2.2.7 其实 一个 cornjob 也被自动 产生了
用来 renew the cerificate 

![](image/Pasted%20image%2020240312001735.png)


![](image/Pasted%20image%2020240312001751.png)

![](image/Pasted%20image%2020240311151141.png)

![](image/Pasted%20image%2020240311151157.png)


本质是 cerbot -q renew 
![](image/Pasted%20image%2020240311151908.png)



## 2.3 Get Letsencrypt Wildcard Certificate (Using Letsencrypt Nginx DNS Challenge)


https://www.youtube.com/watch?v=cvGFTuZ2TRo&list=PLiMWaCMwGJXlQj0BFEwew0fvhOPr833GS&index=2‘

这个 实验的最终效果是 get a wildcard certificate, 像下面一样  

![](image/Pasted%20image%2020240311152232.png)

![](image/Pasted%20image%2020240311152332.png)


----

in Ubuntu
1 install nginx 
sudo apt -y install nginx 

2 install cerbot 
sudo apt -y install cerbot 

3 only dns-01 challenge it able to get wildcard certificate 

4 sudo cerbot certonly --manual
去产生 wildcard certificate 
To just obtain the certificate without installing it anywhere, the certbot certonly (“certificate only”) command can be used.



下面 这里要输出你想给出的 domain name 
![](image/Pasted%20image%2020240311153133.png)

这里会让你 depoly a dns txt record under the xxx name with a specific value yyyy 
![](image/Pasted%20image%2020240311153222.png)

![](image/Pasted%20image%2020240311153709.png)


---

或者也可以 
![](image/Pasted%20image%2020240312002926.png)



5
run reverse dns lookup  
![](image/Pasted%20image%2020240311153822.png)

![](image/Pasted%20image%2020240311153840.png)

![](image/Pasted%20image%2020240312003801.png)



6
查看 生成的 wildcard certificate 

![](image/Pasted%20image%2020240311154854.png)

可以看到 certificate 的位置  和 pricate key 的位置 



7 
configure the nginx server with the wildcard certificate 
先创立文件夹和文件 
![](image/Pasted%20image%2020240311155714.png)

index.html 
![](image/Pasted%20image%2020240311155733.png)

8
setup for site in nginx 
![](image/Pasted%20image%2020240311155751.png)

![](image/Pasted%20image%2020240311155804.png)


setup a soft link to enable this configuration 
![](image/Pasted%20image%2020240311160024.png)


verify the nginx configuration by execute `nignx -t`
![](image/Pasted%20image%2020240311160153.png)



8 configure the conf file for damain devopsbyexample.io in nginx 

![](image/Pasted%20image%2020240312004338.png)

![](image/Pasted%20image%2020240312004436.png)



9 
restart nginx 
![](image/Pasted%20image%2020240311160439.png)

check the status 
![](image/Pasted%20image%2020240311160500.png)


10 
在 domain server 中 填入 一项 新的 record (A type,   servername to ip adrresee )

![](image/Pasted%20image%2020240311215536.png)

 
用 dig 去访问它   看能不能 resolve 他
![](image/Pasted%20image%2020240311215602.png)


![](image/Pasted%20image%2020240311215626.png)



---

## 2.4 Renew Certificate: automate renewal process the challenge 

https://www.youtube.com/watch?v=81TKQIl1rCU&list=PLiMWaCMwGJXlQj0BFEwew0fvhOPr833GS&index=3

every time you renew your certificate, you have to update your txt records， 因为新的 certificate 产生， 就会产生新的 对应的 txt 的 token , value 会变化
the let's encryt generate certificate for you for 90 days ， so you have to renew the txt record manually every 60 days

Ubuntu Server has to create a challenge and create the txt record in DNS provider. 
Then The let's encrypt will query your domain for txt records and verify TXT record is present. 
When it can prove it. let's encrypt will generate and give the Ubuntu Server the certificate. 
This Certificate is only 90 days vaild. every 60 days you have to update that certificate. That means you have to go through this process all over again: create a challenge and update the txt records again. 
==The way we automate this, we will delegate TXT DNS queries to our own DNS server== 

![](image/Pasted%20image%2020240311222116.png)


---

 acme-dns: A simplified DNS server with a RESTful HTTP API to provide a simple way to automate ACME DNS challenges.

use the acme dns server to automate the renew certificate and txt record 
dns provider will outsource our dns server in the ubuntu

1. we will create a ubuntu server. We can ust Nginx, Apache, or any other webserver, and we will install an acme-dns server there as well. 
2. ==When we reqeust a certificate with cerbot form Letsecrypt, we outsource TXT DNS queries from our DNS hosting provider. not the public DNS provider== . it can be any provider  
3. acme-dns-client will use Rest API provided by the acme-dns server to put correct TXT records. 
4. When Letsecrypt verifies that TXT is present with the correct value, it will return a certificate to our certbot 

![](image/Pasted%20image%2020240311222315.png)






### 2.4.1 demo 

1 产生 wildcard certificate 
![](image/Pasted%20image%2020240311222455.png)


 2 
可以看到 产生了 一个 类似的 txt reocrd, 但是为 cname type 

![](image/Pasted%20image%2020240311222645.png)

![](image/Pasted%20image%2020240311222709.png)


3 
在 dns provider ， 没有 txt record, 但是有对应的 Cname。 It is the benefit of acme dns server 
![](image/Pasted%20image%2020240311222807.png)



3 查询 certifacte 的  的内容 

![](image/Pasted%20image%2020240311223122.png)

![](image/Pasted%20image%2020240311223432.png)


4 certificate decoder

将证书复制进去 
![](image/Pasted%20image%2020240311223543.png)



### 2.4.2 Create EC2 Instance, security group 

inbound rules 
![](image/Pasted%20image%2020240311224320.png)


dns (udp) is for small request 
DNS (TCP) is for request when the data size exceeds 512 


### 2.4.3 install acme DNS server on EC2 instance 


download acme-dns 

![](image/Pasted%20image%2020240311225109.png)


update acme dns configuration: sudo vim /etc/acme-dns/config.cfg 

![](image/Pasted%20image%2020240311230647.png)

![](image/Pasted%20image%2020240311225449.png)

![](image/Pasted%20image%2020240311225515.png)

![](image/Pasted%20image%2020240311231739.png)


![](image/Pasted%20image%2020240311231824.png)

![](image/Pasted%20image%2020240311231858.png)


---
在 dns provider 中 
加入 两个 record


### 2.4.4 Install acme-dns-client
 
![](image/Pasted%20image%2020240311232734.png)


### 2.4.5 install certbot 

![](image/Pasted%20image%2020240311233000.png)


### 2.4.6 Get Letsencrypt Wildcard Certificate

1 
register domain name in acme-dns-server 
localhost:8080 就是 acme-dns-server 本身 

![](image/Pasted%20image%2020240311233224.png)

![](image/Pasted%20image%2020240311233407.png)



2  get certificate 

![](image/Pasted%20image%2020240311233847.png)



![](image/Pasted%20image%2020240311234045.png)
![](image/Pasted%20image%2020240311234105.png)



---
setup cornjob 

![](image/Pasted%20image%2020240311234134.png)

选2

![](image/Pasted%20image%2020240311234207.png)


![](image/Pasted%20image%2020240312010425.png)

