
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

1 安装 nginx 
![](image/Pasted%20image%2020240311150047.png)

---

2 看 nginx 里面的配置 

![](image/Pasted%20image%2020240311150133.png)

![](image/Pasted%20image%2020240311150202.png)



verify the nginx configuration 
![](image/Pasted%20image%2020240311150330.png)

---


3  用 cerbot 来 delegate a domain 

![](image/Pasted%20image%2020240311150600.png)

--nginx ist the plugin to add functionallity to be able to locate that serial name block in nignx and also modify  the nignx configuration 

![](image/Pasted%20image%2020240311150725.png)

---


3 再次查看 nginx 的配置 

![](image/Pasted%20image%2020240311150824.png)

可以看出来 新增了很多项

![](image/Pasted%20image%2020240311150837.png)



4 其实 一个 cornjob 也被自动 产生了
用来 renew the cerificate 

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



5
run reverse dns lookup  
![](image/Pasted%20image%2020240311153822.png)

![](image/Pasted%20image%2020240311153840.png)

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


9 
restart nginx 
![](image/Pasted%20image%2020240311160439.png)

check the status 
![](image/Pasted%20image%2020240311160500.png)


10 
在 domain server 中 填入 一项 新的 record (A type,   servername to ip adrresee )



 
