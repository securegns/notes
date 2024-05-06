Linux security book - https://linux-training.be/linuxsec.pdf
### Linux security courses
- https://www.udemy.com/course/linux-security/learn/lecture/4649564
- https://www.udemy.com/course/hackproof-your-server-linux-security/learn/lecture/7318084#overview

### Linux security notes

### Linux/Bash notes
- Important bash/linux topics shared - https://www.youtube.com/playlist?list=PLShDm2AZYnK1SdG3dufPdCqk08sOahUBP
- Interesting playlist with interesting topics - https://www.youtube.com/playlist?list=PL5--8gKSku174EnRTbP4DzU2W80Q1vqtm
- Centos is open source fork of red hat enterprise linux
## Notes

##### Exit codes and chaining commands
- ```$?``` saves the exit code of the previous command. ```0```=success, ```1```=failed, A non-zero (1-255) exit status indicates failure.
- ```cat /etc/passwd && cat 123``` returns code 1, ```cat /etc/passwd || cat 123``` returns 0.
- If 1st command success, do not run 2nd command ```cat 123 || touch 123```
- If 1st command fail, run 2nd command ```touch 123 && cat 123```
- ```#!/bin/bash```, if we are not sure of bash path use ```#!/usr/bin/env bash```
- Make a file executable ```chmod +x ./file.sh```
- 
