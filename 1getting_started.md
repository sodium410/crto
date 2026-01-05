**Red Teaming**: 'Red teaming' is a type of security exercise designed to assess how well an organisation can detect and respond to cyber attack.  
is the process of using tactics, techniques and procedures (TTPs) to emulate a real-world threat,  
with the goal of measuring the effectiveness of the people, processes and technologies used to defend an environment.  

The goal of a red team is to achieve an operational objective that has been pre-agreed with the client.   
This will vary between clients, but will often entail gaining access to business-critical systems or data.  

Tactics(The Why): Overall goal.. Initial acces, Persistence, Data exfiltration  
Techniques(The How): Phishing, exploiting vulnerabilitie ..  
Procedures(The What): step-by-step actions and tools used to execute eg.. creds read from LSASS using mimikatz  

**Blue teams**: SOC teams to defend..  

**Adversary Emulation**: Emulate a specific threat,  
focused evaluation of their capabilities against a threat actor that is more likely to target them  
mirror the known TTPs of that actor as closely as reasonably possible  

**Adversary Simulation**: Simulate a hypothetical threat, leverage unkown or unique TTPs      
This provides an organisation with a broader evaluation of their capabilities and can highlight lesser-known blind spots  

Refrain from harmful actions without the consent of our clients.  
However, when in doubt, seek advice from team lead or client contact.  

**Stealth and OPSEC**: Operations security used to describe the likelihood of actions being observed by enemy intelligence  
OPSEC only becomes a significant concern during adversary simulation, rather than emulation.  
Because when emulating an adversary, the red team is constrained to a specific set of TTPs.  
However, during a simulation, the red team's goal is to achieve the operational objective before getting caught by the blue team,  
so OPSEC becomes a lot more important.  

**Cyber Kill Chain**: Described each phase an attacker must go through to compromise a target  
OR different phases soc can kill the attack  

Reconnaissance-->Weaponisation-->Delivery-->Exploitation-->Installation-->Command&Control-->Action on Objectives  

**MITRE ATT&CK**: widely-used framework - a knowledge base of various phases of attack lifecycle by TTP  

**Engagement planning**:  
Goal planning: what ability does a threat have...  
to gain physical/remote access, to gain elevated access, to move freely in network, to identify/access sensitive info, to initiate a reaction from org  
Rules of Engagement: authorized targets, engagement objectives, any other restrictions  
Do's: log all actions, understand your tools, be aware   
Dont's: use untested or pre-compiled tools, use unencrypted channels for C2, exfiltrate restricted data, disable security controls  
