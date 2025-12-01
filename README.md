# Active-Directory-Home-Lab
<br />
<h2>Description</h2>
A full Active Directory Domain setup utilizng Windows Server 2025 domain controller and Windows 11 Enterprise workstation on VMWare Workstation with custom users, groups, and GPOs to simulate enterprise structures. The project will also demonstrate an attack on the enterprise system, followed by mitigation/remediation tactics and procedures. The project is ocean-themed :))
<h2>Disclaimer</h2>
This repository contains solutions and writeups for various Capture The Flag (CTF) challenges. The content is provided strictly for educational, research, and self-training purposes. DO NOT use this information to attack, compromise, or disrupt any system, network, or application without prior, written authorization. Engaging in unauthorized access is illegal and unethical.
<h2>Project Overview</h2>
<b>Microsoft Evaluation Center ISO Files</b>
<p></p>
  - Windows Server 2025 - https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025
<br />
  - Windows 11 Enterprise - https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise
<br />
<br />
Just enter a bunch of dummy data for the user information requirements and you should be able to download the ISOs afterwards. 
<br />
<br />
The screenshots below will cover main components configured on the AD. The project currently work in progress with setting up the environment as a CTF. Attacks and mitigations will be present soon!!
<br />
<h2>Project Screenshots</h2>
<p align="center">
After installion, we have the server running, however, it's not a Domain Controller yet. Here, I'll add the needed domain services roles, create a forest with a root domain called 'deepblue.local', and rename the server to 'DeepBlue-DC': <br/>
<img src="images/DomainServer.png" height="80%" width="80%" alt=""/>
<img src="images/pcname.png" height="80%" width="80%" alt=""/>
<img src="images/domainservices.png" height="80%" width="80%" alt=""/>
<img src="images/newdomain.png" height="80%" width="80%" alt=""/>
<br />
<br />
<br />
As you can see, the server is now a domain controller based on the login screen: <br /r>
<img src="images/Domainset.png" height="80%" width="80%" alt=""/>
<br />
<br />
<br />
Next, I'll add AD Certificate Services & Certificate Authority for cryptographic trust and security within our domain: <br /r>
<img src="images/certificateservices.png" height="80%" width="80%" alt=""/>
<img src="images/certificateauthority.png" height="80%" width="80%" alt=""/>
<br />
<br />
We can now create a group called '_ADMINS' and add our own domain admin 'a-ptrident': <br /r>
<img src="images/domainadmin.png" height="80%" width="80%" alt=""/>
<br />
<br />
Next, we can add Remote Access and Routing for any kind of remote connectivity for employees. I've configured DHCP to assign an ip within the 172.16.0.100-200 range with a DNS gateway of 172.16.0.1 (DC IP) The idea was to allow internet access on workstations through the domain controller: <br /r>
<img src="images/RAS.png" height="80%" width="80%" alt=""/>
<br />
<br />
Setting up the Windows 11 Enterprise workstation called 'Abyss-WS1'. I'll have it connect to our deepblue.local domain: <br /r>
<img src="images/windows11.png" height="80%" width="80%" alt=""/>
<img src="images/workstationadded.png" height="80%" width="80%" alt=""/>
<br />
<br />
I created a couple of shares that will end up being part of the CTF later on. In this screenshot, I created a share called 'IT Share' to showcase its creation on the domain, but later changed it to 'Public': <br /r>
<img src="images/networkshare.png" height="80%" width="80%" alt=""/>
<br />
<br />
</p>
<h2>Offensive Simulation : FOR EDUCATION PURPOSES ONLY - </h2>
I launched a man-in-the-middle called 'Responder' that will send LLMNR/NBT-NS poisoning requests to the DC and workstations in hopes of an 'employee' would try to access a share that doesn't exist. As a pentester, you would run this attack in the morning or at lunch where a lot of network traffic is present. Employees, especially early morning, are prone to making typos or misclicks when attempting to access shares. For example, employee may type 'Pyblic' on a share named 'Public' and that'll trigger a response. <br /r>
<br />
<br />
<p align="center">
Nmap scans for target validation and port scanning: <br /r>
<img src="images/nmapscanDC.png" height="70%" width="70%" alt=""/>
<img src="images/nmapscanWS.png" height="70%" width="70%" alt=""/>
<br />
<br />
Here I'll launch responder on a kali linux machine. The first image will show a user account attempt to access the 'Public' share misspelled, and responder will capture the forced authenticated NTLM hash: <br /r>
<img src="images/firstindicator.png" height="70%" width="70%" alt=""/>
<img src="images/responder.png" height="70%" width="70%" alt=""/>
<br />
<br />
We can run hashcat to brute-force a match against common passwords found in rockyou.txt. Next, we can validate the credentials and shares using netexec: <br /r>
<img src="images/hashcat.png" height="70%" width="70%" alt=""/>
<img src="images/usercredsshares.png" height="70%" width="70%" alt=""/>
<br />
<br />
Accessing the share and discovering a file called 'pass.txt' (simulate information disclosure via plaintext passwords): <br /r>
<img src="images/passfile.png" height="70%" width="70%" alt=""/>
<img src="images/lateral.png" height="70%" width="70%" alt=""/>
<br />
<br />
And, again, abusing information disclosure by discovering domain admin creds in a txt file: <br /r>
<img src="images/domaincomp.png" height="70%" width="70%" alt=""/>
<br />
<br />
</p>
<h2>Mitigation Steps:</h2>
<p>
Since the initial compromise was a Link Local Multicast Name Resolution (LLMNR) poisoning scheme, we want to disable LLMNR by adding a GPO and enforcing it on the domain. On the workstation, we can run 'gpoupdate /force' to update policies on it: <br /r>
<img src="images/gpomitigation.png" height="80%" width="80%" alt=""/>
<br />
<br />
We can also remove any kind of plaintext files that could be leveraged against our domain and urge employees to refrain from storing credentials as such. In addition, we could set up a PAM solution and apply complexity rules and requirements so that user's aren't prone to making passwords as easy as 'Password1', making it harder to crack. 
</p>
