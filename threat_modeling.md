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

