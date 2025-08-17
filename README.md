<h1>SOC Lab Project – Monitoring BITS Jobs with Splunk and Atomic Red Team</h1>

<h2>Description</h2>
This project demonstrates how I use a Windows 10 VM, Splunk, and Atomic Red Team (ART) T1197 tests. It includes:

- Configuring Windows Security auditing and PowerShell Script Block Logging.

- Installing and setting up Atomic Red Team safely for BITS Job simulations.

- Forwarding Windows Security logs to Splunk for analysis.

- Creating queries and alerts in Splunk to detect BITS job activity.

- Demonstrating real-time monitoring and SOC-style alerting.

This repository serves as a hands-on example of endpoint monitoring and threat simulation for educational and SOC training purposes.
<br />


<h2>Prerequisites</h2>

- <b>Windows 10 Pro installed on Vituralbox/VMWare</b> 
- <b>Splunk Enterprise Downloaded on Windows VM</b>
- <b>Splunk Universal Forwarder Installed on Windows VM</b>
- <b>Configure Windows Security Auditing, Module Logging, PowerShell Script Block Logging, Process Creation Auditing/Termination</b>
- <b>Atomic Red Team (ART) Installed<b>

<h2>Environments Used </h2>

- <b>Windows 10 Pro</b>

<h2>Before Starting, ensure the following:</h2>

- <b>Windows logs/index (For Mine is endpoint_bits) are configured to be forwarded to Splunk via the Universal Forwarder.</b> 
- <b>Splunk has an index (windows_logs/endpoint_bits) where events from Windows are being collected.</b>
- <b>--> (gpedit.msc) and ensure the correct audit policies are enabled</b>

<h2>Installing Atomic Red Team (ART)</h2>

<p align="center">
Execution Policy Change: <br/>
<img src="https://i.imgur.com/Q4LJ5Z5.png" height="80%" width="80%" alt="Execution Policy Change"/>
<br />
<br />
Installing ART in PowerShell:  <br/>
<img src="https://i.imgur.com/iVldkfx.png" height="80%" width="80%" alt="Installing ART in PowerShell"/>
  
  - "IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam"

<p align="center">
<img src="https://i.imgur.com/Ig6asUP.png" height="80%" width="80%" alt="Excluding ART Folder"/>

- Go to Windows Security and Exclude the Atomic Red Team (ART) Folder.
<p align="center">
<img src="https://i.imgur.com/OhOBrUa.png" height="80%" width="80%" alt="Installing ART in PowerShell"/>

  
<br />

  <h2>Configuring Module Logging & Powershell Script Block Logging</h2>
  
<br />
<p align="center">
Enabling Module Logging:  <br/>
<img src="https://i.imgur.com/JJ3Rwij.png" height="80%" width="80%" alt="Enabling Module Logging"/>
<br />
<br />
    
  <p align="center">
Enabling PowerShell Script Block Logging: <br/>

<p align="center">
<img src="https://i.imgur.com/wDK86Om.png" height="80%" width="80%" alt="Enabling PowerShell Script Block Logging"/>
<br />
<br />
________________________________________________________________________________________________________
  
 <h2>Configuring Local Event Logs</h2>
<br />
<p align="center">
Local Event Log Collection in Splunk:  <br/>
<img src="https://i.imgur.com/S58munR.png" height="80%" width="80%" alt="Configuring Local Event Logs"/>
<br />
<br />
  
- <b></b>Enable Application, Security, System for Event Log Collection</b>

 <p align="center">
 Configuring Event Log Collection in inputs.conf: <br/>
<img src="https://i.imgur.com/9TtJccv.png" height="80%" width="80%" alt="Configuring Event Log Collection in inputs.conf"/>
<br />
   
   - Go inside the Splunk Universal Forwarder, edit the inputs.conf, and add the 2 following.
   
   - [WinEventLog://Microsoft-Windows-Windows Defender/Operational] disabled = 0 index = endpoint_bits
     
   - [WinEventLog://Microsoft-Windows-PowerShell/Operational] disabled = 0 index = endpoint_bits
<br />
<br />

________________________________________________________________________________________________________

<h2>Testing BITS Jobs & Creating Alerts</h2>
<br />
<p align="center">
BITS Jobs:  <br/>
<img src="https://i.imgur.com/IFlYY5L.png" height="80%" width="80%" alt="BITS Jobs"/>
<br />
<br />
    
- <b>Background Intelligent Transfer Service (BITS) jobs are Windows processes used to transfer files in the background. In cybersecurity, BITS can be abused by attackers to move files or execute commands stealthily, making them a key technique for SOC analysts to monitor in endpoint detection and response workflows.</b>

 <p align="center">
 Testing BITS Jobs: <br/>
<img src="https://i.imgur.com/BFdQN86.png" height="80%" width="80%" alt="Testing BITS Jobs"/>
<br />
<br />
  
<p align="center">
  BITS Jobs in Splunk:  <br/>
<img src="https://i.imgur.com/kDGhg3A.png" height="80%" width="80%" alt="BITS Jobs in Splunk"/>
<br />
<br />
  
<p align="center">
  Creating Alerts For BITS Jobs:  <br/>
<img src="https://i.imgur.com/UTOK7DU.png" height="80%" width="80%" alt="Creating Alerts For BITS Jobs"/>
<br />

- <b> index="endpoint_bits" sourcetype="WinEventLog:Security" | search "bitsadmin.exe" OR "AtomicRedTeam_T1197"

<br />
<br />

<p align="center">
  Triggered Alerts For BITS Jobs:  <br/>
<img src="https://i.imgur.com/wIsdkzR.png" height="80%" width="80%" alt="Triggered Alerts For BITS Jobs"/>
<br />


- https://github.com/galbachjr
________________________________________________________________________________________________________
  

</p>



