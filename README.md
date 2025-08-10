## Phishing Analysis
This project is a email analysis lab designed to simulate the investigation of a suspicious phishing email. The goal is to identify indicators of compromise (IOCs) and determine whether the email is malicious. I used a sample .eml file, basic OSINT tools, and manual inspection techniques to analyse headers, links, and attachments.

### Inspecting Email Header
Firstly the email is said to be from paypal, howver the return path of the email did not resemble that of a paypal employee acount or anything to do with the company. 

<img width="600" alt="image" src="https://i.imgur.com/tvyNrEq.png">

When putting the email header to MXToolbox, we found a range of problems. Firstly, the SPF is not authenticated meaning the sender is not authorised to send on behalf of that domain and islikely a spoofed source IP.ALso, spam filters will give it a high spam score. Furthermore, the DKIM failed which indicates that the message could have been modified in transit or the sender didn't sign it properly. This makes it easier for attackers to forge content without detection. Last of all the DMARC failed and as a result the email likely was sent to spam or blocked entirely.

<img width="600" alt="image" src="https://i.imgur.com/UxQ96DT.png">


### Analysising embedded links

When hovering over the embedded website link, i saw the complete url on the side and it seemed to be a sub domain of google. To check if the url was suspicious, i used the VirusTotal website and pastd the url link. The url was flagged once out of 97 different techniques and was flagged as a phishing attempt by well known detection  engines. 

<img width="600" alt="image" src="https://i.imgur.com/mmANGZg.png">

I then re-analysed it and the same result occurred. As VirusTotal has a community section, myself and others could insert information about this urls and most comments also seem to agree that the url was malicious and was apart of a phishing campaign.

<img width="600" alt="image" src="https://i.imgur.com/npnNytZ.png">

### Configured OpenVAS to Perform First Unauthenticated Scan against Vulnerable VM
 <img width="730" alt="image" src="https://i.imgur.com/MHjlsqp.png">
1. Logged into OpenVAS and navigated to New Host.
2. Added the Client VM Private IP Address.
3. Created a new target from the host, and named it "Azure Vulnerable VMs".
4. Took note of the credentials as i was adding SMB credentials later.
5. Created a new Task:
    "Scan - Azure Vulnerable VMs"
   - Scan Targets: "Azure Vulnerable VMs"
6. Saved the Task.
7. Started the "Scan - Azure Vulnerable VMs" Task.
8. Once the scan was finished, I clicked the date under "Last Report" to see the results.
9. Took note of the Tabs, especially the "Results" tab.

### Made Configurations for Credentialed Scans (Within VM)
<img width="508" alt="image" src="https://i.imgur.com/6sKjVhH.png">

1. Disable the Windows Firewall.
2. Disable User Account Control.
3. Enabled the Remote Registry.
4. Set Registry Key:
   - Launch Registry Editor (regedit.exe) in "Run as administrator" mode.
   - Navigate to HKEY_LOCAL_MACHINE hive.
   - Open SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System key.
   - Create a new DWORD (32-bit) value with the following properties:
     - Name: LocalAccountTokenFilterPolicy
     - Value: 1
   - Close Registry Editor.
5. Restart the VM.

### Made Configurations for Credentialed Scans (OpenVAS)

1. Went to New Credential.
2. Name / Comment: "Azure VM Credentials".
3. Allowed Insecure Use: Yes.
4. logged in and saved
5. Go to Configuration > Targets > CLONE the Target we made before.
6. NEW Name / Comment: "Azure Vulnerable VMs - Credentialed Scan".
7. Ensure the Private IP is still accurate.
8. Credentials > SMB > Select the Credentials we just made: Azure VM Credentials.
9. Save.

### Executed Credentialed Scan against Vulnerable Windows VM

<img width="861" alt="image" src="https://i.imgur.com/JeWrDnW.png">

1. Within Greenbone / OpenVAS, go to Scans > Tasks.
2. cloned the "Scan - Azure Vulnerable VMs" Task and Edited it.
3. Name / Comment: "Scan - Azure Vulnerable VMs - Credentialed".
4. Targets: Azure Vulnerable VMs - Credentialed Scan.
5. Save.
6. Clicked the Play button to launch the new Credentialed Scan and wait for it to finish.

### Remediated Vulnerabilities
<img width="799" alt="image" src="https://i.imgur.com/mThmLAX.png">

1. Logged back into vulnerable VM.
2. Uninstall Adobe Reader, VLC Player, and Firefox.
3. Restart the VM.
4. Re-initiate the "Scan - Azure Vulnerable VMs - Credentialed" scan and observe the results.

### Verify Remediations
<img width="1120" alt="image" src="https://i.imgur.com/SI9VYHn.png">

 After this, there were no longer Vuln
