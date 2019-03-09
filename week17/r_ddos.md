## ARTS

### Share

## DDOS攻击会破坏服务器吗？


### Understanding DDoS Attack

*Distributed Denial of Service (DDoS) attacks are designed to interrupt a website’s availability. The objective of a DDoS attack is to prevent legitimate users from accessing a website. For a DDoS attack to be successful, the attacker needs to send a number of requests that the victim server can’t handle. The other way a successful attack occurs is when the attacker sends bogus HTTP requests to the server.*

### **What is the Aim Behind a DDoS Attack?**

The main aim of an attacker that is leveraging a Denial of Service (DoS) attack method is to disrupt a website availability:

* The website can become slow to respond to legitimate requests.

* A DDoS attack requires an attacker to gain control of a network of online machines in order to carry out an attack.


DoS Assault Types

1. **Application layer attacks** can be either DoS or DDoS threats that seek to overload a server by sending a large number of requests requiring resource-intensive handling and processing. Among other attack vectors, this category includes HTTP floods, slow attacks (e.g., Slowloris or RUDY) and DNS query flood attacks.

The size of application layer attacks is typically measured in requests per second (RPS), with no more than 50 to 100 RPS being required to weaken most mid-sized websites.

2. **Network layer attacks** are almost always DDoS assaults set up to clog the “pipelines” connecting your network. Attack vectors in this category include UDP flood, SYN flood, NTP amplification and DNS amplification attacks, and more.

Any of these can be used to prevent access to your servers, while also causing severe operational damages, such as account suspension and massive overage charges.

DDoS attacks are almost always high-traffic events, commonly measured in gigabits per second (Gbps) or packets per second (PPS). The largest network layer assaults can exceed 200 Gbps; however, 20 to 40 Gbps are enough to completely shut down most network infrastructures.

![图片](https://uploader.shimo.im/f/hfPnOCJgx9gmFijw.png!thumbnail)

How do DDoS attack work?

The theory behind a DDoS attack is simple, although attacks can range in their level of poise. Here’s the basic idea. A DDoS is a cyberattack on a server, service, website floods it with Internet traffic. If the traffic overcome the target, its server, service, website, or network is rendered inoperable.

Network connections on the Internet consist of different layers of the Open Systems Interconnection (OS) model. Different types of DDoS attacks focus on particular layers. A few examples:

* Layer 3, the Network layer. Attacks are known as Smurf Attacks, ICMP Floods, and IP/ICMP Fragmentation.

* Layer 4, the Transport layer. Attacks include SYN Floods, UDP Floods, and TCP Connection Exhaustion.

* Layer 7, the Application layer. Mainly, HTTP-encrypted attacks.

### What are the symptoms of a DDoS attack?

DDoS attacks have conclusive symptoms. The problem is, the symptoms are so much similar like other issues you might have with your computer, ranging from a virus to a slow Internet connection that it can be hard to tell without professional diagnosis. The symptoms of a DDoS are:

* Slow access to files, either locally or remotely.

* A long-term inability to access a particular website.

* Internet disconnection.

* Problems accessing all websites.

* Excessive amount of spam emails.


Most of these symptoms can be hard to identify as being unusual. Even so, if one or more occur over long periods of time, you might be a victim of a DDoS.

DDoS Attack Trends Spotted in Recent…

**A New Recent Attack Vector Emerges — Memcached**


InFebruary of this year, a new DDoS attack vector came on the scene — memcached server attacks via UDP. Memcached is a database caching system that is used to speed up database-driven websites, decreasing load time. Memcached attacks work similarly to other amplification attacks, sending requests for information to a server that responds with a larger amount of data, magnifying traffic volume. For a 15-byte request, a response as large as 750 kB can be sent. According to [Imperva](https://www.imperva.com/blog/new-ddos-amplification-attack-vector-via-memcached-servers/), memcached server attacks can result in an amplification factor of 9,000x and more. These types of attacks have produced the largest DDoS attacks yet.

**Highest Trend in Attack Size**

Use of the memcached attack vector resulted in the breaking of a new record for the largest DDoS attack in March of this year. [NetScout](https://www.netscout.com/arbor-ddos) (Arbor Networks) recorded a 1.7 Tbps attack on one of their clients, who remains unnamed. Another massive memcached attack also happened in March — a 1.35 Tbps attack on **GitHub’s** website. Both of these attacks exceeded the size of the Mirai botnet attacks in 2016. According to NetScout, DDoS maximum attack size has increased globally by 174%. However, it is worth mentioning that the majority of DDoS attacks remain small, with approximately 95% of attacks being relatively small at under 5 Gbps in volume (Corero, 2018).

### How do I protect myself?

Protecting against DOS attacks can be pretty simple. Victims can block the attacker’s IP address at firewall or ISP level, depending on the severity of the attack.

Security tools and enterprise products are in exist which can block ICMP or SYN attacks.

DDOS attacks are far harder to guard against, and there are various methods. One of these involve having the ISP trash all incoming traffic to the webserver, legitimate or not. This can help save and secure you client’s personal information.

Other ways are to use SYN cookies or HTTP reverse proxies, depending on the different type of attack.

### A final Conclusion on DDOS..

DDOS attacks happen simply every day. If you have assets online and fear a DDOS attack, the best thing you can do it consult a web-security expert. They may be pricey, and the software you need could be even more expensive, but you never know when you might need protection.


[https://hackernoon.com/will-ddos-attack-break-the-servers-b5995676a286](https://hackernoon.com/will-ddos-attack-break-the-servers-b5995676a286)
