# Debugging Kernel
Steps
* Enable kernal debugging and get key - ```bcdedit debug on``` ```bcdedit net hostip:192.168.0.101 port:55xxx``` ```bcdedit /dbgsetting net hostip:192.168.0.101 port:55xxx```
* Now open windbg -> Attach kernel -> enter port and the key from above command

# Windows Host security features
