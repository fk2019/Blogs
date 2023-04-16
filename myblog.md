#+TITLE: What happens when you type `https://google.com` and press `Enter` ?
#+DESCRIPTION: How the web works
#+AUTHOR: Francis Kamau
#+EXPORT+FILE_NAME: /home/my_tasks/how_the_web_works.html
#+SETUPFILE: e:/My files/org-html-themes/readtheorg-local.setup
#+OPTIONS: num:nil toc:nil
* Introduction
Have you ever wondered how you receive pictures of cats, upcoming movies,
or items in your favorite online store when you search on google on your computer
or smartphone? Well, the internet is an interesting technology that powers
today's world and understanding how it works is important.

To get a better grasp of how the web works, I will cover the following topics:
- [[DNS request]]
- [[TCP/IP]]
- [[Firewall]]
- [[HTTPS/SSL]]
- [[Load-balancer]]
- [[Web server]]
- [[Application server]]
- [[Database]]

** DNS request
Domain Name System or DNS in short is a system that converts the human-readable
urls to machine-readable inputs. You probably do not know the actual IP address
of google.com or any other since it also keeps changing. This is where DNS comes
in. It maps the domain name you typed into its asscoiated IP address.

Let's take a quick trip around the internet to find out exactly how the IP address
is obtained by DNS. Upon pressing `Enter`, the browser checks with the OS if
their is a cache of google.com. If not, the resolver, which is usually the ISP 
(Internet Service Provider) checks with the root domain. The root domain does
not have subdomains. For example, google.com is a root domain but www in
www.google.com is the subdomain. If root domain server does not have the requested
IP, it will point the resolver to the Top-Level-Domain (TLD) server.

A list of all valid top-level domains is maintained by the IANA and are updated time to
time. You can find the list [[https://data.iana.org/TLD/tlds-alpha-by-domain.txt][here]]. The TLD, in this case .com server will point the
resolver to the authoritative nameserver. The authoritative nameserver, as the
name suggets, is the ultimate server that holds the IP of requested domain. It
contains the A record of the domain. Upon receiving the IP, the resolver sends it the browser that is the able to receive
and load the requested page e.g. an HTML page to your screen. The browser first parse the HTML file
recognizing any <link> or <script> elements. It then builds a DOM tree for HTML,
CSSOM for css and executes parsed javascript and during this process, a visual
representation of the page appears on the monitor.
 It is also important to note that while the resolver is moviing around requesting for information,
it saves each information so that it does not make many trips
searching for the same domain. So, if you request google.com after a few seconds
or minutes, your browser will quickly load the page since it has that cache.
Another point to note is the amount the resolver keeps this information or Time
To Live (TTL). If say the TTL for a domain is 1 day, that is how long the
resolver will cache the domain before making another trip.

** TCP/IP
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
model (OSI) since TCP/IP and other technologies are described in it.The OSI model is a *conceptual* model that describes the functions of a telecommunication
system without any regard to their basic structure and technology. In other words,
it is a model that helps us understand the complex telecommunication system by defining
it in several layers. It does not specify how different technologies should be made.
Furthemore, one protocol can be found in several layers. For instance, in TCP/IP,
if we are talking about IPs, then we are operating at Network layer. On the other
hand, ports will operate at Transport layer.

There are 7 layers:
| Application  |7
| Presentation |6
| Session      |5
| Transport    |4
| Network      |3
| Data link    |2
| Physical     |1

Since TCP/IP defines how computers communicate to transfer data, the transport
layer is present here.

** Firewall
The core function of a firewall is to monitor incoming and outgoing traffic in a
network system. Firewalls can be found in diffrent OSI layers. For instance,
Next Generation Firewall is an *application(layer 7)* firewall that is more sophisticated
that normal firewalls since it can also conduct deep-packet inspection, inspect encrypted
traffci, and has an intrusion prevention system. A traditional firewall that simply filters
IP and ports at layers 3 and 4. It is also important to note that the configuration of a firewall
will determine how it can be described in the model.

** HTTPS/SSL
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
lives in the *application* layer.

SSL may be terminated at the load balancer to allow web servers to do their job.
This is because decrypting and encrypting keys expends system resources and given that
web servers are tasked with huge requests, extending HTTPS/SSL into them might slow
them if they are not fully equipped to handle such kind of load. As such, the load-balancer
communicates with webserves via HTTP in a protected network of firewalls.

** Load-balancer
Earlier, I mentioned that certificate can be installed on a load-balancer. But
what exactly is a load-balancer? Well, as the name suggest, if you thought of it
as a system that balances loads, then your are right.

In web infrastructure, designing a network system that meets varying demands
is critical in ensuring users are served without delays and that existing systems
are not overworked.

Let us assume that you have a single web server for your site. When traffic
increases and more users start visiting your page, your server will be overwhelmed
leading to failures in serving pages. Additionally, it this current system provides
a single-point-of-failure (SPOF) since if the server fails, every other thing beomes
unavailable and users will not be served. This is where load-balancers come in.

At its core, a load-balancer ensures that no single web server does all the heavy-
lifting. A number of algorithims exist that define how the balancing should be done.
HAProxy is a load-balancer that uses round-robin algorith by default. Round-robin
alternates between web servers such that if you have two web servers, the browser
will be served by the first server then the second and back to the first server.

As you can see, a load-balancer ensures that the there is high availability of
resources since the load of serving pages and other files is fairly distributed.

** Web server
For anyone to understand what a web server is, it is important to first grasp the
meaning of a server. Put it simply, a server can be a device, physical or virtual
or a computer program that provides functionality for other programs or devices knwon as clients.
As such, a web server is a software that provides web functionality to clients. Wen servers
and other servers are usually found in data centers whos function is to manage servers.
A web server serves a number of files including HTML pages to clients. The files being
served by a web server as sent to the user just as they are without modification.
That is, the files are static. If there is any modification of files or the server
must interact with another application such as a database, then the server becomes an application server.
Examples of popular web servers include Nginx and Apache.

Servers utilize process for normal operations. As noted on the [[https://www.gnu.org/software/libc/manual/html_node/Processes.html#Processes][GNU website]], a process
is the primitive unit for system resource allocation. Also, a process executes a
program and multiple process can execute the same program. Child processes are created
from a parent process and can start or stop without affecting its parent process.
Servers such as Apache spawn child processes to manage the number of requests made by users.
If a limit that a child process executes is reached, anoter child process is created
to execute the same program.

** Application server
As noted above, an application server provides additional functionality compared to
a web server. If a request requires information from a databse, the web server sends
that dynamic request the application server that then fetches from the database. Response
is then sent back to the web server and finally to the browser. An application server
sits between a web server and a database and serves applications.

** Database
A database is a collection of information stored and organized in such a way that
it can be easily retrieved, updated, and managed. One can store information about users in a
file but it would be a hassle retrieving and managing records of millions or bilions of users.
That is where databases come in because a good database is scalable. A good database should be *ACID*. That is;
- Acidity: If a transaction happens to fail, it woukd appear as if it never happened
- Consistency: data abides by a defined set of rules and if not, then the transaction
  fails
- Isolation: when two operations are done simultaneously, the result would be as if
  they were ran consecutively. If a file storage was used, isolation would not be
  achieved since the latter operation will fetch an outdated record.
- Durability: data is persistently stored. It is not lost when a server is shutdown and
  restarted again.

*** Types of Databases
A relational database is the most common type of database. A relational database links
records of tables together in the sense that a relation is established. Structure Query
Language (SQL) syntax is used various operations that interact with the database.
Examples of relational databases icnlude MySQL, PostgreSQL. NoSQL (non-relational)
databases include MongoDB and Redis among others. MongoDB offers a document database
that stores information in JSON format on disk while Redis is an in-memory type od database
known for its strong caching and low latency speed because data is store in-memory rather
than on hard disk.

In web infrastructure design, a system could have a master-slave architecture to provide
load balancing and ensure high availability. The master database usually has the write
access and the slave ones read access. Records in the master are copied over to the slaves
but read operations are only done on slave databases so as not to overwhelm the master
database.

SQL has been around for many years and will continue to do so because of its role in
relational databases. The language may be startling if you are new but it becomes
way exciting when you learn how powerful it is. From retrieving any amounts of record to
performing different types of joins on different databases, SQL is one powerful language.

Another way of interacting with a relational database involves the use of Object
Relation Mappers (ORM). ORM libraries such as SQLAlchemy allows softaware engineers to interact
with databases without constructing SQL syntax. By using the awesome language of Python,
one can easily abstract a database on a high-level. And since everything in Python is
an object :), records stored in tables are turned into objects. For instance, I wrote the below
code to print all states in a database by order of their id using SQL and Python with
the latter applying SQLAlchemy library.

#+BEGIN_SRC sql
SELECT id, name FROM states ORDER BY id;
#+END_SRC

#+BEGIN_SRC python
    Base.metadata.create_all(engine)
	session = Session(engine)
	for state in session.query(State).order_by(State.id).all():
	    print("{}: {}".format(state.id, state.name))
	session.close()
#+END_SRC

* Conclusion
Let us recap what we have read today. Upon pressing Enter to browse the internet,
a request is sent to obtain the IP and a response is provided after a successful DNS query.
DNS is important as it converts domain names to their respective IP addresses.TCP/IP is a suite of protocols that define rules
of how computers over the internet and private network should follow for proper communication.
An OSI model has 7 layers and is conceptual and helps us understand the functions of network system.
HTTPS/SSL ensure that network traffic is secure through the use of certificates.
A load-balancer ensures high availability of functionality of servers by switching requests
to different configured web/app servers. A web server serves static pages while an application
server serves dynamic content. Finally, we looked at databases and saw the different
database technologies used to store and retrieve information. With all this in mind,
one should be noow able to understand how the fundamentals of how web works.
