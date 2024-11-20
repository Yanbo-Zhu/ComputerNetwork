
tcp contains youtube  报文内容中包含 youtube 

ip.addr == 172.217.22.174

tcp port 443 or tcp port 80 or host 192.168.0.72 


# 1 protocol filter 

analyze -> enable protocol 

![](image/Pasted%20image%2020241028085300.png)


# 2 regex的使用 
https://osqa-ask.wireshark.org/questions/22376/filtering-with-a-regular-expression/

frame matches "(attachment|exe|zip)"
![](image/Pasted%20image%2020241120203314.png)

http.request.uri matches "(attachment|exe|zip)"

![](image/Pasted%20image%2020241120203349.png)

`frame matches "User-Agent: \\r\\n$"` 
`dns.qry.name matches "berlin$"`用$ 代表 berlin 后面还有只
![](image/Pasted%20image%2020241120203627.png)
