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



2. On April 14, 2026 at 2:17 PM, a process was started on the computer named edr-enroll-vm. The user account labuser launched the program Tor Browser (tor-browser-windows-x86_64-portable-15.0.9.exe) from the Downloads folder located at:
C:\Users\labuser\Downloads\

<img width="852" height="105" alt="image" src="https://github.com/user-attachments/assets/7652f221-63e9-4d6c-ac58-13e42769fa16" />

<img width="1655" height="327" alt="image" src="https://github.com/user-attachments/assets/833860f1-93d8-4e1d-af0b-26dfbf4e999a" />
