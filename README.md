# Nessus Vulnerability Management Lab

<h2>Description</h2>
This lab goes through the process of creating a Windows 10 host virtual machine on VMWare Workstation 17 Player to scan for vulnerabilities using the Tenable Nessus Essentials scanner. We first perform a scan without using credentials, and the second scan is a credentialed scan. This is to see how many more vulnerabilities show up after a credentialed scan. The third scan is performed after installing an old deprecated version of Firefox, and the fourth scan is performed after deleting Firefox and updating the OS.
<br />


<h2>Utilities</h2>

- <b>Tenable Nessus Essentials</b>

<h2>Environments</h2>

- <b>VMWare Workstation 17 Player</b>
- <b>Windows 10</b> (21H2, 22H2)

<h2>Lab walk-through:</h2>
<ol>
 <h3><b>PART 1 - SET UP NESSUS & VM ENVIRONMENTS</b></h3><br/>
 <li>Install VMWare Workstation Player
  <ul><li>From link https://www.vmware.com/content/vmware/vmware-published-sites/us/products/workstation-player/workstation-player-evaluation.html.html.html.html</li></ul>
 </li>
 <br />
 <br />

 <li>Create installation media for Windows 10 iso file
  <ul><li>From link https://www.microsoft.com/en-us/software-download/windows10</li></ul>
 </li>
 <br />
 <br />
 
 <li>Create account & install Nessus Essentials
  <ol>
   <li>From link https://www.tenable.com/products/nessus/nessus-essentials</li>
   <li>Register for an activation code & check for an email by Tenable</li>
   <li>Click “download Nessus” button in the email</li>
   <li>Click on the “Nessus” option & click the Download button</li>
   <li>Install the msi file from the Downloads folder</li>
   <li>Go to this link: http://localhost:8834/WelcomeToNessus-Install/welcome</li>
   <li>Select “Register for Nessus Essentials”, skip & enter the activation code from the email
    <ul><li>Installation will take some time, go to the next step while this is going in the background</li></ul>
   </li>
  </ol>
 </li>
 <br />
 <br />

 <li>Setup Windows 10 Virtual Machine on VMWare
  <ol>
   <li>Open the VMWare Workstation Player application</li>
   <li>Click on the “Player” top left tab, go to the “file” row & click “New Virtual Machine…” (CTRL+N)</li>
   <li>Select the option “Installer disc image file (iso)”, browse for your Windows 10 iso file & click next</li>
   <li>Make sure the name & location is appropriate, and click next</li>
   <li>Set the maximum disk size to 50GB, click next, click ”Customize Hardware…” & set RAM to 4GB</li>
   <li>Go to Network Adapter & select “Bridged” option</li>
   <li>Click on “Configure Adapters” & uncheck everything with the word “Ethernet” in it</li>
   <li>Click close, click finish & wait for the VM creation to complete</li>
   <li>Go through the Windows 10 installation process (Select Windows 10 Pro & select Custom installation with all defaults)</li>
   <li>Ensure there's a network connection on the Windows 10 VM (test it with this command: ping “IP address” -t)
    <ol>
     <li>Go to the start menu, type “wf.msc”, click on it & click “Windows Defender Firewall Properties”</li>
     <li>For the first 3 tabs of the window, go to the Firewall state dropdown menu & select “Off”, click ok</li>
    </ol>
   </li>
  </ol>
 </li>
 <br />
 <br />

 <li>Setup Nessus Essentials
  <ol>
   <li>In “My Scans”, click the link “Create a new scan” & select “Basic Network Scan”</li>
   <li>Set up the Network Scan (input IP address from Windows 10 VM), click save</li>
   <br />
   <p align="center">
    Setting up Nessus scan: <br/>
    <img src="https://i.imgur.com/INGcdrN.png" height="50%" width="50%" alt="Nessus Lab Steps"/>
   </p>
   <br />
   <li>Go to the scans, click on the play button “Launch” on the scan we just created and wait for the scan to complete</li>
  </ol>
 </li>
 <br />
 <br />

 <li>Setup Virtual Machine Network Access
  <ol>
   <li>Go to the start menu of the VM, type “services.msc” & click on the application</li>
   <li>Go down to “Remote Registry” and double click on it</li>
   <li>Go to “Status type”, select “Automatic” & click apply</li>
   <li>Click the “Start” button under “Service status” & close the services window</li>
   <li>Go to the start menu of the VM, type “advanced sharing settings” & click on it</li>
   <li>Make sure under “File and printer sharing”, the setting is at “Turn on file and printer sharing”</li>
   <li><b>***THIS STEP IS NOT GOOD TO DO IN ANY OTHER CONTEXT, FOR LAB PURPOSE ONLY***</b>
    <ol>
     <li>Go to the start menu of the VM, type “User account control” & click on it</li>
     <li>Turn off the setting, click ok & click yes</li>
    </ol>
   </li>
   <li>Go to the start menu of the VM, type “regedit” & click on it</li>
   <li>Browse to “HKEY_LOCAL_MACHINE” -> “SOFTWARE” -> “Microsoft” -> “Windows” -> “CurrentVersion” -> “Policies” -> “System”</li>
   <li>Right-click, go to “New” & click on “DWORD (32-bit) Value” & name it “LocalAccountTokenFilterPolicy”</li>
   <li>Double click on the newly created value, set the Value data to “1” & close the window</li>
   <li>Restart the virtual machine</li>
  </ol>
 </li>
 <br />
 <br />

 <li>Configure Nessus Credentialed Scan
  <ol>
   <li>Go to the Nessus Scans section, click the checkbox for the scan</li>
   <li>Click on “More” dropdown & click “Configure”</li>
   <li>Go to the credentials tab, enter your username & password, click the “Save” blue button</li>
   <li>Go back to the scans page & run the scan again
    <ul><li>Big difference between credentialed scans vs uncredentialed scans</li></ul>
   </li>
   <li>Click on the vulnerabilities tab to check vulnerabilities</li>
  </ol>
 </li>
 <br />
 <br />
 
 <h3><b>PART 2 - LAB EXERCISE (REMEDIATE VULNERABILITIES)</b></h3>

 <li>Install Deprecated Software on VM
  <ol>
   <li>You can install any deprecated software, but for this example we went with an old version of Firefox: https://ftp.mozilla.org/pub/firefox/releases/3.6.12/win32/en-US/</li>
  </ol>
 </li>
 <br />
 <br />

 <li>Scan VM again
  <ol>
   <li>Go to http://localhost:8834/WelcomeToNessus-Install/welcome</li>
   <li>Go to the scans section & launch the scan we created earlier</li>
   <li>A lot more vulnerabilities were found (only due to old version of Firefox)</li>
  </ol>
 </li>
 <br />
 <br />

 <li>Remediate Vulnerabilities
  <ol>
   <li>Go to the “Remediations” tab & analyze the suggestions</li>
   <li>Remove deprecated software
    <ol>
     <li>Go to Programs & Features (Type the run command “Win + R”, type in “appwiz.cpl” & press enter)</li>
     <li>Right-click on the deprecated software (in this case, the old version of Firefox) & uninstall</li>
    </ol>
   </li>
   <li>Update Windows
    <ol>
     <li>Go to the start menu of the VM, click on settings, click “Update & Security” & wait</li>
     <li>Update, restart</li>
     <li>Repeat steps 1 & 2 until no updates appear</li>
    </ol>
   </li>
  </ol>
 </li>
 <br />
 <br />

 <li>Scan again
  <ol>
   <li>Go to http://localhost:8834/WelcomeToNessus-Install/welcome</li>
   <li>Go to the scans section & launch the scan we created earlier</li>
   <li>Once the scan is complete, click on it & analyze it</li>
  </ol>
 </li>
</ol>

<h2>Lab results:</h2>
<p align="center">
Scan 1 screenshots:  <br/>
<img src="https://i.imgur.com/pLOo81z.png" height="80%" width="80%" alt="Nessus Lab Steps"/>
<br />
<br />
Scan 2 screenshots:  <br/>
<img src="https://i.imgur.com/NnaQR1o.png" height="80%" width="80%" alt="Nessus Lab Steps"/>
<br />
Scan 2 remediation screenshot:  <br/>
<img src="https://i.imgur.com/XcY9iLP.png" height="80%" width="80%" alt="Nessus Lab Steps"/>
<br />
<br />
Scan 3 screenshot:  <br/>
<img src="https://i.imgur.com/2oyXtxX.png" height="80%" width="80%" alt="Nessus Lab Steps"/>
<br />
Scan 3 remediation: <br/>
<img src="https://i.imgur.com/5l3fqGz.png" height="80%" width="80%" alt="Nessus Lab Steps"/>
<br />
<br />
Scan 4 screenshot:  <br/>
<img src="https://i.imgur.com/EJTgL7F.png" height="80%" width="80%" alt="Nessus Lab Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
