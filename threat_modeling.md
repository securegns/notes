Book - Threat Modeling: Designing for Security - Book by Adam Shostack

Threat modeling is all about making more secure software

### Reasons to Threat Model
- Find Security Bugs Early
- Understand Your Security Requirements
- Engineer and Deliver Better Products
- Address Issues Other Techniques Won’t

### The Four-Step Framework
1. What are you building?
2. What can go wrong with it once it’s built?
3. What should you do about those things that can go wrong?
4. Did you do a decent job of analysis?
In simple words - Model System -> Find Threats -> Address Threats -> Validate

*The Right Way Is the Way That Finds Good Threats, Avoid a religious warand find a way that works for you.*

When you learn to play the violin, you don’t start with the most beautiful violin music ever written. You learn to play scales, a few easy pieces, and then progress to trickier and trickier music.

*Elevation of Privilege, a serious game designed to help you start threat modeling*

Diagrams are a good way to communicate what you are building

Trust boundaries - Adding boundaries to show who controls what is a simple way to improve the diagram.

### Trust broundries can include ... 
- Accounts (UIDs on unix systems, or SIDS on Windows)
- Network interfaces
- Different physical computers
- Virtual machines
- Organizational boundaries
- Almost anywhere you can argue for different privileges

### TRUST BOUNDARY vs. ATTACK SURFACE
The Trust boundry is where the permissions change to perform any actions, Attack surface is all the points where an attacker can try to enter or extract data from a system

### What Can Go Wrong
- Use methodologies like STRIDE CIA DRAID

### STRIDE
- Spoofing
- Tampering
- Repudiation
- Information Disclosure
- Denial of Service
- Elevation of Privilege

### Tips for Identifying Threats
- Start with external entities
- Never ignore a threat because it’s not what you’re looking for right now - Redundancy in what you fi nd can be tedious, but it helps you avoid missing things.
- Focus on feasible threats - Prioritizing more likely and actionable risks over less probable scenarios 

### Addressing Each Threat
- Mitigating threats
- Eliminating threats - Transferring threats, Accepting the risk

### Addressing Spoofing
| THREAT                  | TARGET                                    | MITIGATION STRATEGY                    | MITIGATION TECHNIQUE                                                      |
|-------------------------|-------------------------------------------|----------------------------------------|---------------------------------------------------------------------------|
| Spoofing a person       | Identification and authentication         | Usernames, real names, or other identifiers | - Passwords<br>- Tokens<br>- Biometrics<br>- Enrollment/maintenance/expiry   |
| Spoofing a “file” on disk | Leverage the OS                          | Full paths<br>Checking ACLs<br>Ensuring that pipes are created properly | Cryptographic authenticators<br>Digital signatures or authenticators      |
| Spoofing a network address | Cryptographic                          | - DNSSEC<br>- HTTPS/SSL<br>- IPsec                                             |
| Spoofing a program in memory | Leverage the OS                      | Many modern operating systems have some form of application identifier that the OS will enforce |
