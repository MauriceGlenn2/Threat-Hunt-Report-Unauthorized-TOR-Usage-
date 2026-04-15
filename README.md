# Threat Hunt Report Unauthorized TOR Usage

# Scenario:
Management suspects that some employees may be using TOR browsers to bypass network security controls because recent network logs show unusual encrypted traffic patterns and connections to known TOR entry nodes. Additionally, there have been anonymous reports of employees discussing ways to access restricted sites during work hours. The goal is to detect any TOR usage and analyze related security incidents to mitigate potential risks. If any use of TOR is found, notify management.


# High-Level TOR related IoC Discovery Plan
1. Check DeviceFileEvents for any tor(.exe) or firefox(.exe) file events
2. Check DeviceProcessEvents for any signs of installation or usage
3. Check DeviceNetworkEvents for any signs of outgoing connections over known TOR ports

# Steps Taken
1. Searched the DeviceFileEvents table for any file containing the string “tor” and discovered that the user “labuser” downloaded a Tor installer. Subsequent activity appears to have resulted in multiple Tor-related files being copied to the desktop, as well as the creation of a file named “tor-shopping-list.txt” on the desktop. These events began at:
 2026-04-14T19:14:35.220112Z

 <img width="1140" height="181" alt="image" src="https://github.com/user-attachments/assets/a99b5d45-d543-496d-a293-9b498df9e598" />

<img width="1128" height="616" alt="image" src="https://github.com/user-attachments/assets/981aa234-780f-4af0-9874-1915963fea5b" />
<br><br><br>



On April 14, 2026, at 2:17 PM, a process was initiated on the computer named edr-enroll-vm. The user account labuser launched Tor Browser (tor-browser-windows-x86_64-portable-15.0.9.exe) from the following directory:
C:\Users\labuser\Downloads\

An investigation of the DeviceProcessEvents table was conducted to identify any ProcessCommandLine entries containing the string "tor-browser". The results showed that at 2:17:21 PM on April 14, 2026, the user on the edr-enroll-vm device executed the file tor-browser-windows-x86_64-portable-14.0.1.exe from the Downloads folder.

<img width="852" height="105" alt="image" src="https://github.com/user-attachments/assets/7652f221-63e9-4d6c-ac58-13e42769fa16" />

<img width="1655" height="327" alt="image" src="https://github.com/user-attachments/assets/833860f1-93d8-4e1d-af0b-26dfbf4e999a" />
<br><br><br>
Searched the DeviceProcessEvents table for any indication that the user "employee" actually opened the Tor Browser. 
There was evidence that the browser was opened at 2026-04-14T19:18:26.1922652Z.There were several additional instances of firefox.exe (Tor) as well as tor.exe as well.

<img width="578" height="105" alt="image" src="https://github.com/user-attachments/assets/4121d660-f54d-4abe-9e6d-6131fb40cb52" />
<img width="2018" height="681" alt="image" src="https://github.com/user-attachments/assets/7af3be27-4cb9-4e4d-bfcc-0386ff097444" />
<br><br><br>

3. After researching TOR connection ports, on April 14, 2026 at 2:18 PM, the user account labuser on the device edr-enroll-vm successfully established a connection to the IP address 147.189.174.139 on port 9001, which is a known Tor relay port used for routing anonymous traffic through the Tor network. The connection was initiated by Tor Browser, which was manually installed by the user directly on their Desktop, indicating deliberate use of anonymization software.

<img width="1042" height="696" alt="image" src="https://github.com/user-attachments/assets/42fd1beb-1d11-4d1d-aaa2-837d77be3538" />
<br><br><br>

# Timeline

Phase 1 — Download & Installation
2:14 PM — Tor Browser installer downloaded
User labuser on edr-enroll-vm downloaded the Tor Browser installer to C:\Users\labuser\Downloads. Multiple Tor-related files were subsequently copied to the Desktop.
~2:14 PM — "tor-shopping-list.txt" created on Desktop
A file named tor-shopping-list.txt was created on the Desktop, suggesting intentional and premeditated use of the Tor network.

Phase 2 — Execution
2:17:21 PM — Tor Browser installer executed
labuser ran tor-browser-windows-x86_64-portable-14.0.1.exe from the Downloads folder, initiating a portable installation of Tor Browser directly to the Desktop.
2:18 PM — Tor Browser launched — firefox.exe & tor.exe running
Tor Browser was confirmed open. Multiple instances of firefox.exe (Tor's browser shell) and tor.exe were observed running from C:\Users\labuser\Desktop\Tor Browser...

Phase 3 — Network Activity
2:18:59 PM — First successful Tor relay connection established
tor.exe connected to the Tor relay node at 147.189.174.139 on port 9001, a known Tor relay port. This confirms the device successfully joined the Tor network and began routing anonymous traffic.

# Summary
On April 14, 2026, the user account labuser on the corporate device edr-enroll-vm deliberately downloaded, installed, and launched the Tor Browser — a tool designed to anonymize internet traffic by routing it through a series of encrypted relay nodes. The activity began at approximately 2:14 PM when the installer was downloaded and a file named tor-shopping-list.txt was created, suggesting premeditation. By 2:17 PM the installer was executed, and by 2:18 PM the browser was fully operational.

Between 2:18 PM and 2:20 PM, tor.exe established outbound connections to three external IP addresses — 147.189.174.139, 82.165.144.252, and 91.179.145.24 — all over port 9001, a well-known Tor relay port. These connections were associated with three suspicious domains exhibiting characteristics of algorithmically generated domain names (DGA): ghnkvv4vwz4j5lq36k37oz.com, ox55ekf5s55bv2xyx.com, and p6m32l.com. The local Tor SOCKS proxy on port 9150 was confirmed active, meaning all browser traffic was successfully anonymized.

The combination of intentional installation, a suspicious named file, DGA-like domains, and active Tor relay connections on a corporate device constitutes a clear policy violation and a potential security incident requiring immediate escalation to management and further investigation.

# Response Taken
TOR usage was confirmed on endpoint edr-enroll-vm. The device was isolated and the user's direct manager was notified.
