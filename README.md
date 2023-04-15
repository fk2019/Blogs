

# Introduction

Have you ever wondered how you receive pictures of cats, upcoming movies,
or items in your favorite online store when you search on google on your computer
or smartphone? Well, the internet is an interesting technology that powers
today's world and understanding how it works is important.

To get a better grasp of how the web works, I will cover the following topics:

-   DNS request
-   TCP/IP
-   Firewall
-   HTTPS/SSL
-   Load-balancer
-   Web-server
-   Application server
-   Database


## DNS request

Domain Name System or DNS in short is a system that converts the human-readable
urls to machine-readable inputs. You probably do not know the actual IP address
of google.com or any other since it also keeps changing. This is where DNS comes
in. It maps the domain name you typed into its asscoiated IP address.

Let's take a quick trip around the internet to find out exactly how the IP address
is obtained by DNS. Upon pressing \`Enter\`, the browser checks with the OS if
their is a cache of google.com. If not, the resolver, which is usually the ISP 
(Internet Service Provider) checks with the root domain. The root domain does
not have subdomains. For example, google.com is a root domain but www in
www.google.com is the subdomain. If root domain server does not have the requested
IP, it will point the resolver to the Top-Level-Domain (TLD) server.


### TODO There are 13 TLDs in the world and they include .com, .net, etc, and are operated

by independent organizations. The TLD, in this case .com server will point the
resolver to the authoritative nameserver. The authoritative nameserver, as the
name suggets, is the ultimate server that holds the IP of requested domain. It
contains the A record of the domain. Upon receiving the IP, the resolver sends it the browser that is the able to receive
and load the requested page e.g. an HTML page to your screen.
 It is also impoertant to noted while the resolver is mvoing around requesting for information,
it saves each information so that it does not make many trips
searching for the same domain. So, if you request google.com after a few seconds
or minutes, your browser will quickly load the page since it has that cache.
Another point to note is the amount the resolver keeps this information or Time
To Live (TTL). If say the TTL for a domain is 1 day, that is how long the
resolver will cache the domain before making another trip.


## TCP/IP

Now that you have a clear picture of how the browser is able to display a page
when you search for a domain, let us get into the finer details because the DNS
is not the only one system invloved in the internet.

Transimmition Control Protocol/Internet Protocol is a suite of protocols that
define how computers communicate over the internet or private network. Simply put,
they are rules that computers must follow for communication to be established and
successfully executed.

TCP is a reliable protocol used in the network. It is reliable because it
esnures that data packets are not lost during communication isssues. It
provides a 3-way style that first begins a synchronization (SYN), synchronization
and acknowledgment (SYN+ACK), and finally acknowledgment (ACK). Most protocols
such as SSH, HTTP, HTTPS, Telnet etc use TCP. It's counterpart, User Datagram
Protocol (UDP) is unreliable but also critical in the web. For instance,
it used in real-time events such as online games, zoom meetings etc. In such case,
if a packet is dropped during communication, it is not replaced by a similar
one since the exprerience will feel awkward.

While on this topic, let us also understand the Open Systems Interconnection
model (OSI) since TCP/IP and other technologies are defined in it.


### TODO The OSI loosely defines how levels should interact.

There are 7 levels;

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Application</td>
<td class="org-right">7</td>
</tr>


<tr>
<td class="org-left">Presentation</td>
<td class="org-right">6</td>
</tr>


<tr>
<td class="org-left">Session</td>
<td class="org-right">5</td>
</tr>


<tr>
<td class="org-left">Transport</td>
<td class="org-right">4</td>
</tr>


<tr>
<td class="org-left">Network</td>
<td class="org-right">3</td>
</tr>


<tr>
<td class="org-left">Data link</td>
<td class="org-right">2</td>
</tr>


<tr>
<td class="org-left">Physical</td>
<td class="org-right">1</td>
</tr>
</tbody>
</table>

Since TCP/IP defines how computers communicate to transfer data, the transport
layer is present here.


## TODO Firewall


## HTTPS/SSL

HyperText Transfer Protocol Secure (HTTPS) and Secure Socket Layer (SSL)
are used together to secure the communication between the browser and servers.
HTTPS was developed to address the insecurities of HTTP since data over the latter
is visible to sniffing tools. As such, sensitive information such as passwords or
bank details can be retrieved by hackers.

HTTPS/SSL uses certificates issues by a certificate authority. There are a
number of authorities that one must pay but a free one such as Let's Encrypt
is also available. The certificate is installed on a web server or load balancer
and private keys among other keys along the chain of authority are also created.

The server sends the certificate to the browser and the browser verifies the
the authenticity of certificate. A padlock usually appears on the url bar
indicating that the domain is safe. Remember the OSI model above? HTTPS/SSL
lives in the **network** layer.


## Load-balancer

Earlier, I mentioned that certificate can be installed on a load-balancer. But
what exactly is a load-balancer? Well, as the name suggest, if you thought of it
as a system that balances loads, then your are right.

In web infrastructure, designing a network system that meets varying demands
is critical in ensuring users are served without delays and that existing systems
are not overworked.

Let us assume that you have a single web server for your site. When traffic
increases and more users start visiting your page, your server will be overwhelmed
leading to failures in serving pages. Additionally, it this current system provides
a single-point-of-failure (SOF) since if the server fails, every other thing beomes
unavailable and users will not be served. This is where load-balancers come in.

At its core, a load-balancer ensures that no single web server does all the heavy-
lifting. A number of algorithims exist that define how the balancing should be done.
HAProxy is a load-balancer that uses round-robin algorith by default. Round-robin
alternates between web servers such that if you have two web servers, the browser
will be served by the first server then the second and back to the first server.

As you can see, a load-balancer ensures that the there is high availability of
resources since the load of serving pages and other files is fairly distributed.


## Web server

A web server is a computer that provides functionality to clients. A common web
server serves a number of files including HTML pages to clients. The files being
served by a web server as sent to the user just as they are without modification.
That is, the files are static. If there is any modification of files or the server
must interact with another application such as a database, then the
server becomes an application server


## Application server

As noted above, an application server provides additional functionality compared

