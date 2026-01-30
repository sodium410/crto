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

### Payload Security  
When first started, a Cobalt Strike team server will generate a new unique public/private keypair,  
and every stageless payload generated by that server will have the server's public key embedded into it.  
Beacon uses this key to encrypt its metadata when talking to the team server, ensuring that it can only communicate with the team server that it was generated from.  
 In addition, every Beacon uses a unique session key that is used to encrypt/decrypt the tasking/output data exchanged with the team server.  
 That session key itself is transmitted inside the encrypted metadata, so that only the correct team server can encrypt and decrypt C2 traffic for a Beacon.  
 However, due to their small size, stagers do not carry the same security. When executed, the stager will reach out to the host/port that it was configured with to receive the rest of the payload stage. There is no validation by the stager to ensure that it's talking to a legitimate team server, which does make them susceptible to hijacking when first executed.  
 
### OPSEC  

### Generating Payloads  
Beacon payloads can be generated from the paylaods menu  
HTML application --- html+vbscript payload as executable, powershell or vba  
MS Office macro -- macro 
Stager payload generator -- similar to msfvenom  
Stageless payload generator -- 
Windows stager/stageless payload -- exe, dll, service exe  

**Payload flow**  
revisit once you fully understand all the options  

## Interacting with Beacon  
https://www.cobaltstrike.com/support/user-manuals
The blue monitor icon denotes that the Beacon is running in medium-integrity (i.e. standard user privileges).  
When running in high-integrity (i.e. local administrator or SYSTEM privileges), a red monitor icon is shown and the username is appended with an asterisk.  

Beacon identifies itself to the team server by sending its metadata within the C2 protocol.  For HTTP/S, this is somewhere within the GET or POST request such as the URL, a header, or cookie.  The metadata itself is encrypted using the public key of the team server from which the payload was generated.  This prevents a Beacon from communicating with, or being tasked by, a different team server.  

The DNS Beacon behaves slightly differently.  Because of the very limited data bandwidth, it does not provide its metadata on every check-in like the others do.  DNS Beacons therefore initially appear as 'ghost' sessions, with no metadata and a black monitor icon.  Simply interact with the Beacon and issue a command, like checkin, to get its metadata, and the client will populate itself automatically  

To 'interact' with a Beacon, double-click on its row or right-click and select Interact.  This will open a new tab.  
For a list of commands, type help in the text box at the bottom and press enter  
help  
help getuid  
youl understand more when you actially interact with the tool  



