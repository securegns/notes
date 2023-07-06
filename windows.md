# Windows Host security features

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
