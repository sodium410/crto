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
Listener -- payload handler/data plane    
Type of listeners --- egress -- http/s, dns , peer-to-peer  -- smb, tcp, alias - handler in other toolset  
listener name - should be descriptive enough  

stagets are more less secure, more brittle, easier to detect -- hence better to use stageless payloads  


 




  
