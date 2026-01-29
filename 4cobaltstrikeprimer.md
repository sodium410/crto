Cobalt Strike is a market leading command & control framework that provides a post-exploitation agent to simulate stealthy, long-term embedded actors.  
Has built in reporting, capabilities for initial access, priv esc, lateral mov and disguise  

## Components  
**Beacon**: post-exploitation agent that runs on a compromised endpoint  
implemented as a Windows DLL but can be packaged into  exe, ps, shellcode etc  

**Team Server**: Central control & logging system. beacon reports to team server  
**Redirector**: an intermediatory host that sits between beacon and a team server and proxies traffic between the two.  

**Client**: used by RTO to connect to one or more team servers  
you're prompted to enter the details of a team server that you'd like to connect to  
The client will consolidate the data between them all such as listeners and active Beacon sessions.  

Cobalt Strike's design pattern for distributed operations is to stand up dedicated team servers  
for each phase of an engagement such as initial access, post-exploitation, and persistence  

## Listeners  
The first step in configuring Cobalt Strike is to create one or more listeners.   
A listener defines the protocol and parameters by which a Beacon payload will communicate with the team server.  
The protocols provided by Cobalt Strike out-of-the-box are DNS, HTTP, HTTPS, SMB, and TCP.  

### Two types  
**Egress**- egress beacon will communicate directly with the team server. dns/http/s are egress listeners  
**Peer-to-Peer** - routed through another or multiple beacons, but ultimately be linked to egress beacon to reach team server. smb and tcp are p2p listeners  

### HTTP Listener  
The HTTP listener directs Beacon to communicate with the team server via HTTP GET and/or POST requests.  
By default it will use GETs to fetch tasks from the team server and POSTs to send the results back.  
The team server starts a built-in web server to serve the requests.  

**HTTP Listener Settings**  
HTTP hosts --- Ip or domain name of team server or redirector  
Host rotation strategy  -- how to use when more than one http hosts is provided eg: round-robin, random etc  
Max retry strategy -- For example, exit-50-25-1h tells Beacon to increase its sleep time to 1 hour after 25 failed attempts, and then to exit if it reaches 50 failed attempts.  
HTTP host(stager) -- only used by stager payloads, a stager payload can only use 1 single host to fetch full payload stage even if multiple hosts are configured  
Profile -- Malleable c2 profiles  
HTTP port(C2) -- port beacon attempts to connect to the team server on  
HTTP port(bind) -- port that the team server will bind its built-in web server to, if none specified it uses c2 port.  
can do port bending by configuring redirector to listen on 80 c2 and redirect traffic to team server on different port useful when multiple http listeners on same team server  
HTPP host header  --   
HTTP proxy -- proxy to use by beacon  
guardrails -- prevents stageless beacon payloads from running unless speficied criteria has been met. eg: ip of host, username, hostname, domain name etc  
useful in cases where payloads are copied or forwarded outside of target. eg soc analyst trying to analyse payload in sandbox  

### DNS Listener Settings  
The DNS listener directs Beacon to communicate with a team server via DNS requests - specifically A, AAAA, or TXT record lookups.  
The team server starts a built-in DNS server to serve the requests.  
The DNS Beacon checks into the team server by performing an A record lookup for a domain setup in the DNS listener  
The metadata is not sent until the Beacon is tasked with a job.  
There is a simple checkin command which does nothing other than to transmit the Beacon's metadata.  
DNS resolver -- can be system default or can use 8.8.8.8 or any other  

### SMB and TCP Listeners  
listener that does not bind or listen on the team server VM  
requires another Beacon to connect to that named pipe/tcp port and relay traffic between the SMB/tcp Beacon and the team server  
If Bind to localhost only is not checked, the Beacon will bind to 0.0.0.0.  If it is checked, it will bind to 127.0.0.1.  

## Beacon Payloads  
As already stated, most C2 agents are written as a DLL and paired with a loader to load it from memory.   
What was mentioned, is that there is more than one type of loader.  
A prepended loader is overall more stealthy and flexible than a stomped loader  -- read for differences  

### Staged vs Stageless payloads  
It's common for attack frameworks, such as Metasploit and Cobalt Strike to decouple 'exploits' from 'payloads'.  
Imagine an application that has a code execution vulnerability due to a buffer overflow - the 'exploit' is the code that triggers the overflow and the 'payload' is the code you want executed as a result (e.g. Meterpreter or Beacon shellcode).  




