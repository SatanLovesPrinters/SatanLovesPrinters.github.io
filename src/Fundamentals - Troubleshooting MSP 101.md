# Zero to Hero > 02 - Troubleshooting
## 01 - Workstation Support - Initial Steps

|Support Type|Category|Link/Description|
|---|---|---|
| Workstation Support| Networking | [WiFi/NIC Troubleshooting](https://support.microsoft.com/en-us/windows/fix-wi-fi-connection-issues-in-windows-9424a1f7-6a3b-65a6-4d78-7f07eee84d2c) |
| Workstation Support | Windows/Updates | [Troubleshooting Windows Updates Failing to Install](https://answers.microsoft.com/en-us/windows/forum/all/windows-wont-finish-installing-updates/58a736f9-4a0b-4ac8-b6c8-b674aec3a83b) |
| Workstation Support | Windows/O365 | [Share Drives in a Workgroup](https://answers.microsoft.com/en-us/windows/forum/all/share-drives-in-workgroup/74df0b22-373f-4521-9055-eec76bb0f81b) |
| Workstation Support | Windows/O365 | [Removing Synced SharePoint Libraries from File Explorer](https://kb.uwstout.edu/page.php?id=92115) |
| Workstation Support | Windows/O365 | [O365 Message Format - HTML \| RICH \| PLAIN TEXT](https://support.microsoft.com/en-au/office/change-the-message-format-to-html-rich-text-format-or-plain-text-338a389d-11da-47fe-b693-cf41f792fefa) |
| Workstation Support | Windows/Drivers |[Adobe Print to PDF - Manually Reinstall/Reconfigure](https://helpx.adobe.com/acrobat/kb/add-pdf-printer-manually.html) |
| Workstation Support | Windows | [Rebuilding User Profile Reference Article #2](https://community.spiceworks.com/how_to/121165-re-create-user-profile-windows) |
| Workstation Support | Windows | [Rebuilding User Profile Reference Article #1](https://www.thealfaaz.com/how-to-recreate-a-corrupted-user-profile/) |
| Workstation Support | O365/E-mail | [Header Analysis](https://www.gaijin.at/en/infos/e-mail-header-fields) |
| Workstation Support | | [Troubleshooting TPM](https://docs.microsoft.com/en-us/windows/security/information-protection/tpm/initialize-and-configure-ownership-of-the-tpm) |
| Workstation Support | | [Recovery Lost/Unsaved/Corrupted Documents](https://docs.microsoft.com/en-US/office/troubleshoot/word/recover-lost-unsaved-corrupted-document) |
| Workstation Support | | [Opening Shared Mailboxes in Outlook](https://support.microsoft.com/en-us/office/open-and-use-a-shared-mailbox-in-outlook-d94a8e9e-21f1-4240-808b-de9c9c088afd) |
| Workstation Support | | [Local Printer in RDS Environment](https://www.beaming.co.uk/knowledge-base/techs-using-local-printers-and-drives-in-a-server-connection/) |
| Workstation Support | | [Forcing Applications via Shortcut](https://social.technet.microsoft.com/Forums/ie/en-US/8b3013ff-e60f-490b-bbce-ab5e04d60906/how-to-force-shortcut-to-use-ie-when-not-default?forum=ieitpropriorver) |
| Workstation Support | | [E-mail Stuck in Outbox](https://docs.microsoft.com/en-us/outlook/troubleshoot/synchronization/email-stays-in-outbox-until-manually-send-or-receive) |
| Workstation Support | | [Calculator Application - Windows App Failure](https://docs.microsoft.com/en-us/answers/questions/175770/calculator-does-not-open.html) |
| Sophos Application Support | | [Recover tamper protected system](https://support.sophos.com/support/s/article/KB-000036125?language=en_US#Recover_registry) |
| Mobile Device Support | O365 | [Setup Outlook on Native iOS Mail App](https://support.microsoft.com/en-us/office/set-up-an-outlook-account-on-the-ios-mail-app-7e5b180f-bc8f-45cc-8da1-5cefc1e633d1) |
| Lenovo Device Support | | [Lenovo Support - GEN 9 - Manual](https://download.lenovo.com/pccbbs/mobiles_pdf/x1_carbon_gen9_x1_yoga_gen6_ug_en.pdf) |

#### General List of Commands
```powershell
Start-ADSyncSyncCycle -PolicyType Delta #For Servers Post AD Changes
Test-ComputerSecureChannel -verbose #For Workstations
Get-Service | Where-Object {$_.Status -eq 'Stopped' -and $_.startType -eq 'Automatic'} | #Workstations & Servers
manage-bde -status #Check Bitlocker Drive Encryption
runas /user:DOMAIN\user.name cmd
```
#### CMD / General Workstation Repair
```cmd
sfc /scannow
dism /Online /Cleanup-Image /Restorehealth
chkdsk /f /r
Del /q /f /s %TEMP%\*
```
#### CMD / Admin Control of Printers
```cmd
Rundll32 printui.dll,PrintUIEntry /il 
```
#### CMD / Repairing Time Sync Errors
```cmd
w32tm /config /manualpeerlist:time.windows.com,0x1 /syncfromflags:manual /reliable:yes /update

w32tm/resync
Time 00:00:00 AM/PM
tzutil /s "Eastern Standard Time"
net stop w32time && net start w32time && w32tm /resync
```

### Step-by-Step Troubleshooting (Slow Performance)
1. Update Windows
2. Update BIOS / Manufacturer's Drives / (Lenovo System Update | Dell Command Update | HP Update)
3. ```sfc /scannow && dism /Online /Cleanup-Image /Restorehealth && chkdsk /f /r```
4. Update Windows / Search for Manufacturer's updates again
5. Review Event Viewer / Application & System Logs
6. Clear ANY Application Cache / Reset ANY Interference
    - [OneDrive Reset (if sync related)](https://support.microsoft.com/en-us/office/reset-onedrive-34701e00-bf7b-42db-b960-84905399050c)
        - Open RUN dialog (Windows Key + R) [Select & Run One Below]
        - %localappdata%\Microsoft\OneDrive\onedrive.exe /reset
        - C:\Program Files\Microsoft OneDrive\onedrive.exe /reset
        - C:\Program Files (x86)\Microsoft OneDrive\onedrive.exe /reset
    - Clear Windows %TEMP%
    - Clear Teams Cache (%appdata%\Microsoft\Teams)
        - [MS Learn | Teams Administration | Clear Teams Cache](https://learn.microsoft.com/en-us/microsoftteams/troubleshoot/teams-administration/clear-teams-cache)
    - **Optional** Office 365 - Full Online Repair **only**
    - **Optional** Update Adobe / Update 3rd Party Applications
        - Typically from the "Help > " Interfaces will show an option to update.
        - :warning: This can be an issue for MSP facing readers who have clients on hardset / version specific setups. 
    - **Optional** : Clear Windows Credential Manager
    - **Optional** : Clear Windows Update Folder (**C:\Windows\SoftwareDistribution\Download**)
    - **Optional** : Reinstall OneDrive
    - **Optional** : Uninstall any extra copies of Office 365 or PC Bloatware
    - **Optional** : Filter System & Application Event Logs to Critical-Warning-Error
7. Reboot PC
8. Review Event Viewer
9. Review Workstation Network Connectivity & Interfaces
    - Check Network Interface Priority
    - Check IPConfig/all 
    - Run [speedtest.net](speedtest.net) off & on:
        - LAN/WLAN
        - VPN/No VPN
    - If two NIC's compete at the same priority - A user with a LAN preference could be stuck on an autoconfigured guest WLAN if both interfaces have the same priority (or if one is set lower than the competing NIC)
        - ``` ipconfig /all || Get-NetIPInterface || Set-NetIPInterface -InterfaceIndex <X> -InterfaceMetric <Y>```
10. Sign in as another user profile & test to see if the issue resumes.
    - If Yes: Create a new user profile. 
    - [Workstation Support - Rebuilding User Profile Reference Article #1](https://community.spiceworks.com/how_to/121165-re-create-user-profile-windows)
    - [Workstation Support - Rebuilding User Profile Reference Article #2](https://www.thealfaaz.com/how-to-recreate-a-corrupted-user-profile/)
    - If No: Confirm access to domain
        - ```Test-ComputerSecureChannel -verbose```

### Sledgehammer Solutions - Least Probable, but Randomly Applicable

1. Register misfit applications based on their .dll location. 
    - The idea is to 'resync' the Operating System / File Explorer to the Application. 
    - Think of this like Cache related errors:
        - If the OS is utilizing the wrong trusted information & applying it 'truthfully' there eventually becomes a battle of attrition between two competing 'truthful' processes. This is the phase where Addins/Applications go non responsive for an extended period of time: Utilizing workstation & network resources: Eventually the application will fail & **sometimes an event is triggered / logged to the System / Application logs**
        ```cmd
        regsvr32.exe /s /n /i:user "C:\Users\<UserProfileName>\AppData\Local\Microsoft\TeamsMeetingAddin\<VERSION NUMBER>\x64\Microsoft.Teams.AddinLoader.dll"
        ```
        - Swap out \<UserProfileName> & \<VERSION NUMBER>
            - \<VERSION NUMBER> : Found from **Teams > Ellipses (...) > Settings > (i) About Teams**
2. Registry Editor 
    - Review / Google: "[**Application Name**]" & "[**Error Description**]" & "[**Registry Fix**]"

### Outlook Support
- Recreate Mail Profile
- Full Online Repair
- Registry / GPO Review
- Event Viewer


### Printer Support
```
Adding Printers - General Guidance

DC from VPN for a local printer
Add by TCP/IP & Share (List by IP assigned to)
Log into the printer IP (192.1.x.x) > Setup Scan to Email & Shared Drives

Add Printer Wizard from Admin Panel
- CMD Admin > Rundll32 printui.dll,PrintUIEntry /il
- Try adding as User once more (above only installs the driver as an admin)



verify print driver
verify windows
verify hardware
if (generic/text only doesn't work) = print driver is incorrect
if (not found) = verify its plugged into the network or computer
if (not found) = check Control Panel & see where its being shared from
if (test print) = no test print then Computer & Printer have no connectivity

if (Port) = IP, then its networked
if (Sharing) > in Control Panel > you see "printer on azdc-xxx"
```

Creating a Local Printer Port (Print Server communicates to PC, local access to printer on PC)
- Printers & Scanners
- Add Printer (from User PC)
- Create local printer or network printer
- Create a new port: TCP/IP
- Enter IP of Printer (properties of printer from print server)
- Select the driver
- Complete Installation

Check the Printer Configuration Files [MS - DOC](https://support.microsoft.com/en-us/topic/how-to-troubleshoot-general-printing-problems-in-windows-server-2003-when-you-use-office-products-54489287-c64b-492d-5d52-846711f160b5)
- Delete default printer
- Computer Management
- Services & Applications > Click Services
- Right Click "Print Spooler" = STOP
    - C:\WINDOWS\system32\spool\drivers\w32x86\3
    - delete the files inside the 3 folder
- Computer Management
- Services & Applications > Click Services
- 2x Click Print Spooler: Startup Type:: AUTOMATIC
- After updating printer files, reinstall printer

### Network Support

```
netsh interface show interface
Get-NetIPInterface / Set-NetIPInterface
```

#### DFS Replication - Troubleshooting
```
1)      Connect to Desktop, place Company_Test_Desktop in folder
a.       How long did it take to appear on the below step:

2)      Connect to Laptop on secure WiFi, Connect to NetExtender, (~how long/time to connect)

3)      Connect to Laptop on the Hotspot, Connect to NetExtender, Compare the speeds

4)      Place Company_Test_Laptop into Folder (while on Hotspot) > Compare Speeds
a.       How long did it take to appear:

5)      Have another user in the office place an Company_Test_Office in the folder
a.       How long to see that item in the folder?

6)      Make a change on that document & ask the other user how long it takes to see that change
```

#### General Networking Audit / Confirmation of Issue
```
From the Server
Ping the internal firewall IP
Ping the external firewall IP
Ping an end user internal IP
Ping an external IP (8.8.8.8)
From end user PC
Ping the internal firewall IP
Ping the external firewall IP
Ping an end user internal IP
From my End
Ping the external firewall
Ping the external user IP

Move to a different location > Bandwidth-Test
Does the network have a firewall?
Can you update the firmware? 

Network is Slow Troubleshooting
1. Find Client's External IP & Ping from myWorkPC | Ping my IP from user's PC
2. Look for latency change. Run speedtest.net on WLAN & LAN if possible. On VPN/Off VPN.
3. Ping firewall to check, ping -t to check dropped packets, then tracert firewall
4. (Server) Ping firewall, ping internal IP, ping hostnames. Connect to Firewall > Speedtests again.
5. ipconfig/all for all devices, try a winsock reset/internet connection reset

Continue Scoping Questions: 
- Recent changes? 
- When did this start? 
- Are you the first to report it? How long? Is there a pattern?


If no, move to his machine and run the same tests from his machine to Company IPs. On a mac you'd use the "terminal" and ifconfig commands instead of ipconfig used for windows.
___Always do a power cycle on the modem at first

Questions to ASK:
1. What device are you using to connect?
2. Are other people able to connect?
3. Why are you using this network?
4. And if youve been using this network, any recent changes to it that you know of? 
```

#### Uncommon Firewall Issue / Phone / SMS / SMMS Mobile Devices Unable to Send Outbound on Enterprise Network
```
Can't Send SMS over Network? Autoforward / Unblock any traffic from *.apple.com or exclusive to
https://courier.push.apple.com/
```


### Password Support

- Always connect to VPN or be on office network
- In CW > Companies > Configuration > *mail [configuration type]
    - Click on O365 & look at where the O365 sync server is located
    - Connect to VPN > Ctrl + Alt + Del > Change your password > [Win]+L > Login with New Password

**Password Reset (Out of Sync w/ DC)**

- Connect to the PC
- If (locked) with new set password >Try to unlock with old PW
- Set AD Password to the Expired PW > Reattempt signing into PC
- Connect to the VPN with the newly refreshed old password
- May have to wait 24-48 hours : 
    - Now you can Ctrl + Alt + Del > Change your password > [Win]+L > Login with New Password.