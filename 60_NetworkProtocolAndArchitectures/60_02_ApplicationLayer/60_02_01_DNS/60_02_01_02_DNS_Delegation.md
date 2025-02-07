
https://www.cloudns.net/blog/dns-delegation/

Delegating authority over a subdomain to another organization or another DNS server

DNS Delegation will significantly increase the performance of your DNS network. Thanks to this feature, the whole DNS is so easily scalable. It will reduce the load, increase the speed and redundancy. It is used for almost all subdomains. Knowing how to manage your DNS will increase the performance greatly.

# 1 DNS Zones and Domains

The DNS is a hierarchy structure of domains. It starts from the root domain “.”. Underneath it, there are the TLD domains like “com”, “org”, “net” and so on. Then it is time for the domains of the second level like “co.uk” and so on. All of the domains are hosted using different DNS zones, which are globally distributed and hosted by DNS servers in different international locations.

A Domain is a unique name, like _cloudns.net_, in the DNS. This domain has its DNS zone which hosts all the DNS records for it – [A records](https://www.cloudns.net/wiki/article/10/), [AAAA records](https://www.cloudns.net/wiki/article/11/), [MX records](https://www.cloudns.net/wiki/article/12/) and more.

# 2 **What is DNS Delegation?**


DNS Delegation, also called DNS Zone Delegation, is a process of assigning authority over a domain or subdomain to different DNS servers to keep records updated. When the [Authoritative DNS server](https://www.cloudns.net/blog/authoritative-dns-server/) to which the zone is delegated responds to DNS requests, it recursively resolves the CNAME target or responds with a referral. 
By delegating responsibility over a subdomain to another DNS server, an organization can receive more control over the enabling and disabling services, such as mail exchange, hosted on the subdomain.

main authoritative DNS server  去授权  DNS servers . 让他们去有权利处理 all dns reuqest for xx  subdomain, 比如说 这个 dns server 可以  keep records updated。 
如果 authoritative DNS server 底下的 each zone ist already delegated responfs to dns requests. 那么 这个 authoritative DNS server 的任务就只是  resolves the CNAME target or responds with a referral （转送）. 

![](image/Pasted%20image%2020240305115533.png)


# 3 When do you need DNS Delegation


The DNS gives you the option to separate the namespace into different DNS zones. You can save them, copy them or distribute them to other DNS servers. There are few reasons to do it:

- You would like to load balance by dividing one large zone into more, smaller zones. This will increase the [DNS resolution](https://www.cloudns.net/blog/domain-name-resolution/) and add extra security.
- You desire to delegate management of part of your DNS namespace to another location or department in your organization.
- Use the DNS Delegation for adding various subdomains. Use them for different purposes.
- Delegate control of part of your DNS namespace to another location.
- You can restructure your namespace and make other DNS servers responsible for a part of the whole information.

When you create new DNS zone, you must have delegation records in other zones that point toward the authoritative DNS servers for the new one.

The resource record information of the new [DNS zone](https://www.cloudns.net/blog/master-slave-dns/) will be stored in a DNS server, which will be the primary master for that zone. You can improve the security and duplicate the zone information to another DNS server, such as [Secondary DNS](https://www.cloudns.net/secondary-dns/). It will serve as a backup DNS and will give you additional protection.

# 4 **How do you delegate a subdomain?**

Delegating authority over a subdomain to another organization or another DNS server is a simple process
All you need to do is add [NS records](https://www.cloudns.net/wiki/article/34) for the subdomain into the parent domain, pointing at the delegated server.  
- 在 parent domain 中 加入 add [NS records](https://www.cloudns.net/wiki/article/34) for the subdomain. 这个 records record name 是 subdomain, 指向的taget是 delegated server (就是另一个 dns server )
This means that the trusted server will handle all DNS requests related to the subdomain. 

before delegating it， check that the delegated server is authoritative
However, it is essential to be careful when delegating a subdomain, as any problems with the server or domain management will reflect badly on you. Therefore, it is recommended to use the “dig +norec” command on all the servers to check that the delegated server is authoritative for the subdomain before delegating it.

# 5 **Benefits**

- Provides an additional layer of security as delegated servers can be set up to work as a failover in the event of a system failure on the root server 
- Delegated servers can employ more secure protocols than the root server, such as [DNSSEC](https://www.cloudns.net/dnssec/) (Domain Name System Security Extensions) 
- Allows organizations to create multiple backups, ensuring data and resources are fully protected in the event of an attack 
- Reduces the attack surface by compartmentalizing the authoritative server from its clients, preventing [DNS attacks](https://www.cloudns.net/blog/5-dns-attacks-types-that-could-affect-you/)

# 6 **DNS Delegation example**

DNS zone delegation is a process that allows organizations and companies to delegate authority over a portion of their DNS namespace to another entity. This means an external party can manage a part of a domain’s DNS settings, such as adding or removing A records or [CNAME records](https://www.cloudns.net/wiki/article/13/).

There are many examples where companies delegate part of their DNS space. Such as examples are universities that have delegated a portion of their namespace for managing student email accounts. Or businesses that have delegated their Domain Name System to a third-party service provider, like [ClouDNS](https://www.cloudns.net/), to provide better speed, security, and reliability for their website.

**Here are some examples of what we explained above:**

- **Subdomain delegation** – assigning a DNS server for a specific subdomain such as ‘_email.university.com_’ to be managed separately from the root domain ‘_university.com_’.
- **Domain alias delegation** – For domains in different TLDs (Top Level Domains) such as ‘_example.com_’ and ‘_example.net_’, delegating part of the DNS management to another server, allowing the same DNS records to be shared across both domains.


# 7 **Glue records: The key to effective DNS Delegation**

In the context of DNS Delegation, [Glue records](https://www.cloudns.net/wiki/article/295/) play an indispensable (不可或缺的) role by linking the parent domain with its subdomains. Essentially, these records function by providing the required A and AAAA records that establish a connection between the primary domain and its delegated counterparts. 
Glue records are particularly crucial for resolving what are known as circular dependencies, which arise between domain names and their associated nameservers.

Suggested article: [What DNS Branding is?](https://www.cloudns.net/wiki/article/39/)

To illustrate, let’s consider an example: a main domain named _example.net_ is delegating a subdomain, say, _blog.example.net_, to dedicated nameservers – _ns1.blog.example.net_ and _ns2.blog.example.net_. In this scenario, because these nameservers are under the subdomain they are assigned to manage, Glue records are essential. 
They help in pinpointing the IP addresses of these nameservers. Absent these Glue records, the DNS would find itself in an endless resolution cycle, unable to properly locate the nameservers. 
<mark> Therefore, the parent domain, example.org in this case, must include not only the NS records that indicate delegation but also the A (or AAAA) records that effectively link the nameserver names to their IP addresses, ensuring a smooth and uninterrupted DNS resolution process. </mark>

# 8 **What is reverse DNS zone delegation?**

Reverse DNS zone delegation is a process that allows organizations to delegate responsibility over a [PTR (Pointer) record](https://www.cloudns.net/wiki/article/40/) to a different zone within their domain name space.  比如说 xx.ivu-cloud.local 给 yy.ivu-cloud.local 授权 

It is a two-step process where the organization’s name servers have first delegated the responsibility to handle the DNS records related to its domain names, then the reverse DNS zone.

Reverse DNS Delegation enables organizations to provide faster resolution for DNS requests. Furthermore, it is usually used for security and reliability purposes and for instituting adequate access control policies. By employing rDNS Delegation, organizations can have more control over how their domain and subdomains are accessed and managed.

# 9 **Lame delegation**

Lame delegation occurs in DNS when a nameserver is incorrectly configured or fails to respond authoritatively for a domain it’s listed to serve. This often happens when the NS records in the parent domain point to a server that is not configured for the specified subdomain, resulting in failed or improper [DNS queries](https://www.cloudns.net/wiki/article/254).

For instance, consider a domain, _example.net_, that delegates a subdomain, _blog.example.net_, to a set of nameservers. If one of these nameservers, say _ns1.blog.example.net_, is not correctly configured to resolve queries for _blog.example.net_, or if it’s completely unresponsive, this results in lame delegation. Clients trying to access _blog.example.net_ might experience delays or inability to reach the site, as their DNS queries partially fail due to the non-responsive or misconfigured server.

To prevent lame delegation, it is crucial for domain administrators to regularly verify that all listed nameservers are correctly configured and responsive for all the domains and subdomains they are intended to serve. This includes ensuring that any changes in the DNS configuration are accurately reflected across all relevant nameservers. Regular monitoring and auditing of DNS settings are essential to identify and rectify any instances of lame delegation promptly, thereby maintaining the integrity and reliability of the DNS system.



