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




  
