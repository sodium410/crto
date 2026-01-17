## Introduction  
Cobalt Strike is a market leading command & control framework that provides a post-exploitation agent to simulate stealthy, long-term embedded actors.  
Has built in reporting, capabilities for initial access, priv esc, lateral mov and disguise  

### Components  
**Beacon**: post-exploitation agent that runs on a compromised endpoint  
implemented as a Windows DLL but can be packaged into  exe, ps, shellcode etc  

**Team Serve**r: Central control & logging system. beacon reports to team server  

**Client**: used by RTO to connect to one or more team server  
you're prompted to enter the details of a team server that you'd like to connect to  
The client will consolidate the data between them all such as listeners and active Beacon sessions.  

Cobalt Strike's design pattern for distributed operations is to stand up dedicated team servers  
for each phase of an engagement such as initial access, post-exploitation, and persistence  


