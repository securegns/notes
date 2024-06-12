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

## Addressing Each Threat
- Mitigating threats
- Eliminating threats - Transferring threats, Accepting the risk

### Addressing Spoofing
| THREAT TARGET                                    | MITIGATION STRATEGY                    | MITIGATION TECHNIQUE                                                      |
|-------------------------------|----------------------------------------|---------------------------------------------------------------------------|
| Spoofing a person - Identification and authentication         | Usernames, real names, or other identifiers | - Passwords<br>- Tokens<br>- Biometrics<br>- Enrollment/maintenance/expiry   |
| Spoofing a “file” on disk | Leverage the OS <br>Full paths<br>Checking ACLs<br>Ensuring that pipes are created properly | Cryptographic authenticators<br>Digital signatures or authenticators      |
|  | Cryptographic authenticators | Digital signatures or authenticators |
| Spoofing a network address | Cryptographic                          | - DNSSEC<br>- HTTPS/SSL<br>- IPsec  |
| Spoofing a program in memory | Leverage the OS                      | Many modern operating systems have some form of application identifier that the OS will enforce |

### Addressing Tampering
|THREAT TARGET|MITIGATION STRATEGY|MITIGATION TECHNIQUE|
|-----|----|----|
| Tampering with a file | Operating system | ACLs |
| | Cryptographic | Digital Signatures, Keyed MAC |
| Racing to create a file (tampering with the file system) | Using a directory that’s protected from arbitrary user tampering | ACLs, Using private directory structures (Randomizing your file names just makes it annoying to execute the attack.)| 
| Tampering with a network packet | Cryptographic | HTTPS/SSL, IPsec |
| | Anti-pattern | Network isolation (See note on network isolation anti-pattern.) |

- Tampering with memory: If you’re getting data from a shared JSON file, is it ACLed so only the other process can see it?
- Tampering with network data: man-in-the-middle attack - use SSL and IPsec
- Tampering with networks anti-pattern: Isolating a network to avoid tampering threats is challenging to maintain over time and less effective than anticipated.

 ### Addressing Repudiation Threats
 |THREAT TARGET|MITIGATION STRATEGY|MITIGATION TECHNIQUE|
 |--|--|--|
 | No logs means you can’t prove anything | Log | Be sure to log all the security relevant information |
 | Logs come under attack | Protect your logs | Send over the network, ACL |
 | Logs as a channel for attack | Tightly specified logs | Documenting log design early in the development process |

 - No logs means you can’t prove anything
 - Log machanisims should not be able to be abused by attackers

### Addressing Information Disclosure 
| THREAT TARGET | MITIGATION STRATEGY | MITIGATION TECHNIQUE |
|---|---|---|
| Network monitoring | Encryption | HTTPS/SSL, IPsec |
| Directory or filename | Leverage the OS | ACLs |
| File contents | Leverage the OS | ACLS |
| | Cryptography | File encryption such as PGP, disk encryption (FileVault, BitLocker) |
| API information disclosure | Design | Careful design control, Consider pass by reference or value |

- Names reveal information
- APIs reveal information

### Addressing Denial of Service
|THREAT TARGET|MITIGATION STRATEGY|MITIGATION TECHNIQUE|
|---|---|---|
| Network flooding | Look for exhaustible resources | Elastic resources, Work to ensure attacker resource consumption is as high as or higher than yours |
| | | Network ACLS |
| Program resources | Careful design | Elastic resource management, proof of work |
| | Avoid multipliers | Look for places where attackers can multiply CPU consumption on your end with minimal effort on their end: Do something to require work or enable distinguishing attackers, such as client does crypto first or login before large work factors (of course, that can’t mean that logins are unencrypted). | 
| System resources | Leverage the OS | Use OS settings |

### Addressing Elevation of Privilege
| THREAT TARGET | MITIGATION STRATEGY | MITIGATION TECHNIQUE |
|---|---|---|
| Data/code confusion | Use tools and architectures that separate data and code.| Prepared statements or stored procedures in SQL, Clear separators with canonical forms, Late validation that data is what the next function expects|
| Control flow/memory corruption attacks | Use a type-safe language | Writing code in a type-safe language protects against entire classes of attack | 
| | Leverage the OS for memory protection | Most modern operating systems have memory protection facilities | 
| | Use the sandbox | Modern operating systems support sandboxing in various ways (AppArmor on Linux, AppContainer or the MOICE pattern on Windows, Sandboxlib on Mac OS),Don’t run as the “nobody” account, create a new one for each app. Postfi x and QMail are examples of the good pattern of one account per function |
| Command injection attacks | Be careful | Validate that your input is the size and form you expect, Don’t sanitize. Log and then throw it away if it’s weird |

- Data/code confusion - Ex. XSS code in HTML data

### Validate, Don’t Sanitize
### Trust the Operating System
- The operating system provides you with security features so you can focus on your unique value proposition
- The operating system runs with privileges that are probably not available to your program or your attacker
- If your attacker controls the operating system, you’re likely in a world of hurt regardless of what your code tries to do

### File Bugs
- Someone might use the /admin/ interface without proper authorization
- The admin interface lacks proper authorization controls

## Checking Your Work
### Checking the model
- Is this complete?
- Is it accurate?
- Does it cover all the security decisions we made?
- Can I start the next version with this diagram without any changes?
###### Updating the Diagram 
- Focus on data flow, not control flow.
- Anytime you find yourself needing more detail to explain security-relevant behavior, draw it in.
- Consider adding more detail to break out the various cases(adding cases like sometimes this may happen)
- Data can’t move itself from one data store to another: Show the process that moves it.
- The diagram should tell a story, and support you telling stories while pointing at it.
- Don’t draw an eye chart (a diagram with so much detail that you need to squint to read the tiny print)
### Checking Each Threat
- Did I do something with each unique threat I found?
- Did I do the right something with each threat?
### Checking Your Tests
- Are my security tests in line with the other software tests and the sorts of risks that failures expose?

## Checklists for Diving In and Threat Modeling
##### Diagramming
1. Can we tell a story without changing the diagram?
2. Can we tell that story without using words such as “sometimes” or “also”?
3. Can we look at the diagram and see exactly where the software will make a security decision?
4. Does the diagram show all the trust boundaries, such as where different accounts interact? Do you cover all UIDs, all application roles, and all network interfaces?
5. Does the diagram reflect the current or planned reality of the software?
6. Can we see where all the data goes and who uses it?
7. Do we see the processes that move data from one data store to another?
##### Threats
1. Have we looked for each of the STRIDE threats?
2. Have we looked at each element of the diagram?
3. Have we looked at each data flow in the diagram?
##### Validating Threats
1. Have we written down or fi led a bug for each threat?
2. Is there a proposed/planned/implemented way to address each threat?
3. Do we have a test case per threat?
4. Has the software passed the test?

