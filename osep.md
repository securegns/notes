# Things to learn
C#, Sysinternals, Windows Audit logs, 

# Book Notes
### Windows Registry
Gui tool  -  Registry Editor (regedit)
User level  -  HKEY_CURRENT_USER (HKCU)
Machine level  -  HKEY_LOCAL_MACHINE (HKLM)  -  Needs admin privs to edit

### Client side execution
- **Dropper** requires **Connectback** which is call back to attacker machine to download staged payload
- Staged vs NonStaged payloads in metasploit
##### HTML Smuggling attack
```
var blob = new Blob([data], {type: 'octet/stream'});
var url = window.URL.createObjectURL(blob);
```
```
function base64ToArrayBuffer(base64)
{
 var binary_string = window.atob(base64);
 var len = binary_string.length;
 var bytes = new Uint8Array( len );
 for (var i = 0; i < len; i++) { bytes[i] = binary_string.charCodeAt(i); }
 return bytes.buffer;
}
```
##### Microsoft office

##### Powershell
