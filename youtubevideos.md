Assume breach - Initial access done !!  
https://www.youtube.com/watch?v=i6tsLWrCiGE&list=PLcjpg2ik7YT6H5l9Jx-1ooRYpfvznAInJ&index=1  

Cobalt strike provides options and flexibility for evasion - but not a magic wand  
know your tools and their behavior -- generates observable events  
understand defences in target env  

## Operations  
cobalt strike -- platform for red team ops and adversary simulations - Since Jun 2012  

team server port ---default 50050 --- management/control plane -- required to setup listeners  
This port is not used by beacons, not for callbacks, not for staging, not for payloads.  
It is only used so operators can log in and configure the server.  

lsitener port --- data plane -- used to trasnfer data  
Team Server port is required to create listeners, but it is never used as a listener.   

once listener is setup, team server starts a listener on team server  
the paylaod run use this http listener to talk to team server  

Operator GUI → Team Server port 50050 or 55005 ---> Configure Listeners --->Team Server opens listener ports: 80, 443, 8080, 53, 4444, etc.--->Beacons call back to listener ports  

Beacon ALWAYS sends task output back through the listener port, not the Team Server port.  eg: getuid  

Think of team server port like a checkpoint admin client connecting to checkpoint management server for administration  
can be more than one operator sessions, can see all sessions on client  

team server should be a linux box  

./team-server x.x.x.x password optional_profile  

can use event logs to collaborate with other operators  -- like a livechat  

operator can connect to multiple team servers  
1 for say initial access, 1 for priv esc, 1 for data exfiltration   
and maybe 1 low and slow server for long haul  

Beacon → Listener port  
Operator Client → Team Server port  
operator client is not installed on target just the beacon payload  

beacon logs on team server -- aggressor/logs   

basicaly no gui is installed on team server -- so its the oprator client that enables this gui  

reports menu -- all reports - can merge reports from multiple team servers  

So say i start the team server on gcp, operator client on local kali , and beacon payload on target  
how does beacon payload know which listener to talk to ?  
A Beacon knows which listener to communicate with because the listener information is embedded inside the Beacon payload at generation time.  
This configuration is stored in the Beacon binary itself based on the listener you select when building the payload.  

## Infrasrtucture  
**Listener** -- payload handler/data plane    
Type of listeners --- egress -- http/s, dns , peer-to-peer  -- smb, tcp, alias - handler in other toolset  
listener name - should be descriptive enough  

stagers are less secure, more brittle, easier to detect -- hence better to use stageless payloads  
stagers when size is a limitation  example: stager after landing GET request to downlaod payload in bits  

now by default its stageless  

so if http listener based payload can call it http beacon, if dns based then dns beacon  

scenario...  
say http beacon is dropped on target, what it does -- reach c2 team server Via get and ask do you have any task for me !!  
team server could say no nothing then beacon will goto sleep for defined time  
then beacon wakes up -- sends a get request again -- this time if c2 has something -- the encrypted task data is sent to beacon  
beacon then sends a post request to c2 with results of the tasks performed  
so what happens with malleable profile is define example - instead of 1 big post break it into multiple gets  

A Beacon knows which listener to communicate with because the listener information is embedded inside the Beacon payload at generation time.  
Encryption configuration is stored in the Beacon binary itself based on the listener you select when building the payload.  
Beacon data (AES encrypted)--->wrapped inside HTTP(S)---> Protected by TLS certificate  

if you define a proxy (HTTP/SOCKS) inside a Cobalt Strike HTTP/HTTPS listener, that proxy setting is embedded into the Beacon payload,  
and the deployed Beacon will use that proxy for all of its outbound C2 traffic.  

connect to team server  
start http listener with defaults  
use attacks->scripted web delivery-->select http listener and payload type --> press launch --> payload will be ready on listener waiting -- copy the powershel command run it on target --- this downloads the payload from listener and runs it on target  
now should have a http reverse shell session on operator client !!   

https listeners are same with cert, can use own certs recommended  

**DNS listeners** -- A record requests and 0.0.0.0 in response and task data in response if any pending  
modep command to use dns for c2 check and http for data trasfer  
making team server authoritative name server for the domains and subdomains you control and when nslookup on victim it should reach teamserver  
should have valid ns and a records configured for this to work  
team server ip is no where input on listener, just the fqdn which have A records configured on our dns ns Which points to team server  
works but time taking  

**SMB beacons** --- works on named pipes in local net, window then encapsulates that comms in smb    
smb listener not a real network listener — it is a payload configuration option.  with unique pipename  
initial access say http beacon, can have additional smb beacons that report back to http and each smb beacon can in turn control other smb beacons  

smb beacon--->smb beacon--->httpbeacon  
**TCP beacon** --- very similar to smb but binding to tcp port  
For both tcp and smb beacons the payloads downlaoded from egress listener which is http/dns     

**External C2 listeners** - to make use of 3rd party tools to communicate with cobaltstrike  
just a relay  

**Redirectors**  acts as intermediate proxies between beacon and team servers  
use nginx,apache,cdn,iptables or socat to forward traffic to team server  
socat tcp4-listen:80,fork tcp4:team.server:80   

now allows for multiple egress listeners  
multiple smb tcp listeners ok as well  
can have 2 http listeners on port 80 with differnet profiles -- possible  -- server consolidation  

kali---operator    //is below valid ? yes  
smb beacon--->http beacon--->CDN--->Team server  

2 videos done  










 




  
