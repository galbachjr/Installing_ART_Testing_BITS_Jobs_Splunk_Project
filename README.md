<h1>Testing Alerts in Splunk Using Windows 10 Pro</h1>

<h2>Description</h2>
This project shows how to I set up and test alerts in Splunk by using a Windows 10 computer as the data source. As a beginner, it helped me learn how to collect Windows event logs, create alerts for specific events, and make sure Splunk notifies you when something important happens on your system.
<br />


<h2>Prerequisites</h2>

- <b>Windows 10 Pro installed on Vituralbox/VMWare</b> 
- <b>Splunk Enterprise Downloaded on Windows VM</b>
- <b>Splunk Universal Forwarder Installed on Windows VM</b>
- <b>Sysmon Installed (Optional)</b>

<h2>Environments Used </h2>

- <b>Windows 10 Pro</b>

<h2>Before Starting, ensure the following:</h2>

- <b>Windows logs are configured to be forwarded to Splunk via the Universal Forwarder.</b> 
- <b>Splunk has an index (windows_logs) where events from Windows are being collected.</b>
- <b>--> (gpedit.msc) and ensure the correction audit policies are enabled</b>

<h2>Creating/Testing Alerts</h2>

<p align="center">
Alert Creation: <br/>
<img src="https://i.imgur.com/WX1qd2N.png" height="80%" width="80%" alt="Alert Creation"/>
<br />
<br />
Testing Alerts:  <br/>
<img src="https://i.imgur.com/ktGocc2.png" height="80%" width="80%" alt="Testing Alerts"/>
<br />

  <h2>Alert 4720: Account Creation</h2>

- 1. <b>Event Code 4720 is used to track account creation in Windows.</b>
  
<br />
<p align="center">
Search Query Configuration:  <br/>
<img src="https://i.imgur.com/l66PIvO.png" height="80%" width="80%" alt="Search Query Configuration"/>
<br />
<br />
    
  <p align="center">
Account Creation: <br/>
    
- 2. <b>For this test, I created a user named "splunktest" [-net user {name} {password} /add] </b>
<p align="center">
<img src="https://i.imgur.com/SsaZOG0.png" height="80%" width="80%" alt="Account Creation"/>
<br />
<br />
  
- 3. <b>source="WinEventLog:*" index="windows_logs" EventCode=4720</b>
<p align="center">
  Account Creation 4720 Alert:  <br/>
<img src="https://i.imgur.com/dS2XwXG.png" height="80%" width="80%" alt="Account Creation 4720 Alert"/>
<br />
________________________________________________________________________________________________________
 <h2>Alert 4726: Account Deletion</h2>
<br />
<p align="center">
Search Query Configuration:  <br/>
<img src="https://i.imgur.com/MAGKcKs.png" height="80%" width="80%" alt="Search Query Configuration"/>
<br />
<br />
    
- 1. <b>Event Code 4726 is used to track account creation in Windows.</b>
- 2. <b>source="WinEventLog:*" index="windows_logs" EventCode=4726</b>

 <p align="center">
 Account Deletion: <br/>
<img src="https://i.imgur.com/X76V5Qf.png" height="80%" width="80%" alt="Account Deletion"/>
<br />
<br />

- 3. <b>Deleted the Account I previously made, "splunktest", [-net user {name} /delete] </b>
  
<p align="center">
  Account Deletion 4726 Alert:  <br/>
<img src="https://i.imgur.com/GEYMbKF.png" height="80%" width="80%" alt="Account Deletion 4726 Alert"/>
<br />
________________________________________________________________________________________________________
<h2>Alert 4688: New Process Creation</h2>
<br />
<p align="center">
Search Query Configuration:  <br/>
<img src="https://i.imgur.com/pdPSL2A.png" height="80%" width="80%" alt="Search Query Configuration"/>
<br />
<br />
    
- 1. <b>Event Code 4688 is a Security log event that records whenever a new process is created on the system.</b>
- 2. <b>I only put a few process creations to log that are mostly important.</b>
- 3. <b>source="WinEventLog:*" index="windows_logs" EventCode=4688 New Process Name:"*whoami*" </b> | <b>source="WinEventLog:*" index="windows_logs" EventCode=4688 New Process Name:"*ipconfig*" </b> |<b>source="WinEventLog:*" index="windows_logs" EventCode=4688 New Process Name:"*net.exe*" </b>

 <p align="center">
 Testing 'whoami', 'ipconfig', 'net user' cmds: <br/>
<img src="https://i.imgur.com/1onxMhw.png" height="80%" width="80%" alt="Testing 'whoami', 'ipconfig', 'net user' cmds"/>
<br />
<br />
  
<p align="center">
  New Process Creation 4688 Alert:  <br/>
<img src="https://i.imgur.com/WdowQo5.png" height="80%" width="80%" alt="New Process Creation 4688 Alert"/>
<br />
________________________________________________________________________________________________________
<h2>Alert 4625: Brute Force Attacks</h2>
<br />
<p align="center">
Search Query Configuration:  <br/>
<img src="https://i.imgur.com/lvXm5aD.png" height="80%" width="80%" alt="Search Query Configuration"/>
<br />
<br />
    
- 1. <b>Event Code 4625 is a Security log event and is critical for monitoring unauthorized access attempts or brute-force attacks.</b>
- 2. <b>source="WinEventLog:*" index="windows_logs" EventCode=4625 | stats count by Account_Name, Source_Network_Address | where count > 5</b>

<p align="center">
 Test Brute Force Attack: <br/>
  
- 3. <b>Win + L to lock your computer</b>
- 4. <b>Replicate 5 or more failed attempts to login</b>
  
 <p align="center">
 Brute Force Attack 4625 Alert: <br/>
<img src="https://i.imgur.com/gDo4DxP.png" height="80%" width="80%" alt="Brute Force Attack 4625 Alert"/>
<img src="https://i.imgur.com/VRw0L1Q.png" height="80%" width="80%" alt="Brute Force Attack 4625 Alert2"/>
<br />

- https://github.com/galbachjr
________________________________________________________________________________________________________
  

</p>



