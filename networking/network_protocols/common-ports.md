### Common Port Numbers and Their Associated Services

In networking, **ports** are used to identify specific processes or services on a device. Each service is associated with a unique **port number**, which is used by the **TCP/IP protocol suite** to route traffic to the appropriate application. Below is a list of some of the most commonly used port numbers and their associated services:

| **Port Number** | **Protocol** | **Service**                             | **Description**                                                                 |
|-----------------|--------------|-----------------------------------------|---------------------------------------------------------------------------------|
| **20**          | TCP          | **FTP (File Transfer Protocol) – Data** | Used for transferring files in **active mode** between a client and server.      |
| **21**          | TCP          | **FTP (File Transfer Protocol) – Control** | Used for sending commands and controlling the FTP connection.                   |
| **22**          | TCP/UDP      | **SSH (Secure Shell)**                  | Used for secure remote login and command execution, also used by **SFTP** and **SCP**. |
| **23**          | TCP          | **Telnet**                              | Used for unencrypted, remote login to devices. It is now largely replaced by SSH due to security risks. |
| **25**          | TCP          | **SMTP (Simple Mail Transfer Protocol)** | Used for sending emails between mail servers and clients.                        |
| **53**          | TCP/UDP      | **DNS (Domain Name System)**            | Used for translating domain names to IP addresses and vice versa.                |
| **67**          | UDP          | **DHCP (Dynamic Host Configuration Protocol) – Server** | Used by DHCP servers to assign IP addresses to clients.                         |
| **68**          | UDP          | **DHCP – Client**                       | Used by clients to request IP address assignment from the DHCP server.           |
| **80**          | TCP          | **HTTP (Hypertext Transfer Protocol)**  | Used for unencrypted web traffic (web browsing).                                |
| **110**         | TCP          | **POP3 (Post Office Protocol 3)**       | Used for retrieving email from a mail server to a local client.                  |
| **119**         | TCP          | **NNTP (Network News Transfer Protocol)** | Used for reading and posting articles on Usenet newsgroups.                      |
| **123**         | UDP          | **NTP (Network Time Protocol)**         | Used to synchronize the time on networked devices.                              |
| **143**         | TCP          | **IMAP (Internet Message Access Protocol)** | Used for retrieving and managing emails from a mail server.                      |
| **161/162**     | UDP          | **SNMP (Simple Network Management Protocol)** | Used for network management and monitoring of network devices.                  |
| **194**         | TCP          | **IRC (Internet Relay Chat)**           | Used for real-time text communication over the internet.                        |
| **389**         | TCP/UDP      | **LDAP (Lightweight Directory Access Protocol)** | Used for accessing and managing directory information over a network.           |
| **443**         | TCP          | **HTTPS (Hypertext Transfer Protocol Secure)** | Used for secure web traffic encrypted using SSL/TLS.                            |
| **465**         | TCP          | **SMTPS (SMTP Secure)**                 | Used for sending emails securely over SSL/TLS encryption.                       |
| **514**         | UDP          | **Syslog**                              | Used for logging system events across network devices.                          |
| **587**         | TCP          | **SMTP (with STARTTLS)**                | Used for sending email with TLS encryption, more secure than port 25.            |
| **636**         | TCP/UDP      | **LDAPS (LDAP Secure)**                 | Secure version of LDAP, using SSL/TLS encryption.                               |
| **993**         | TCP          | **IMAPS (IMAP Secure)**                 | Secure version of IMAP, encrypted with SSL/TLS.                                 |
| **995**         | TCP          | **POP3S (POP3 Secure)**                 | Secure version of POP3, encrypted with SSL/TTLS.                                |
| **1433**        | TCP          | **Microsoft SQL Server**                | Used for communication with Microsoft SQL Server databases.                     |
| **1521**        | TCP          | **Oracle Database**                     | Used for communication with Oracle databases.                                   |
| **2049**        | TCP/UDP      | **NFS (Network File System)**           | Used for file sharing between networked devices.                                |
| **3306**        | TCP          | **MySQL**                               | Used for communication with MySQL databases.                                    |
| **3389**        | TCP          | **RDP (Remote Desktop Protocol)**       | Used for remote desktop access to Windows machines.                             |
| **5432**        | TCP          | **PostgreSQL**                          | Used for communication with PostgreSQL databases.                               |
| **5900**        | TCP          | **VNC (Virtual Network Computing)**     | Used for remote desktop control over a network.                                 |
| **6379**        | TCP          | **Redis**                               | Used for communication with the Redis in-memory database.                       |
| **8080**        | TCP          | **HTTP (Alternate)**                    | Often used for web servers as an alternative to port 80.                        |
| **8443**        | TCP          | **HTTPS (Alternate)**                   | Often used as an alternative port for secure HTTPS traffic.                     |

### Summary:

- **Ports** are used to uniquely identify applications or services on a host machine and allow network traffic to be routed correctly.
- **TCP ports** are used for reliable, connection-based communication, while **UDP ports** are used for connectionless, low-latency communication.
- Some common ports include:
  - **80** for HTTP,
  - **443** for HTTPS,
  - **22** for SSH,
  - **25** for SMTP,
  - **53** for DNS,
  - **143** for IMAP. 

These ports are essential for the functioning of various internet services and communication protocols.