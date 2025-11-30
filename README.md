# Active-Directory-Home-Lab
<h2>Description</h2>
A full Active Directory Domain setup utilizng Windows Server 2025 domain controller and Windows 11 Enterprise workstation on VMWare Workstation with custom users, groups, and GPOs to simulate enterprise structures. The project will also demonstrate an attack on the enterprise system, followed by mitigation/remediation tactics and procedures. 
<h2>Project Overview</h2>
<b>Microsoft Evaluation Center ISO Files</b>
<p></p>
  - Windows Server 2025 - https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025
<br />
  - Windows 11 Enterprise - https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise
<br />
<br />
Just enter a bunch of dummy data and you should be able to download the ISOs afterwards. 
<br />
<br />
The home lab is configured to be ** intentionally vulnerable **
This includes (but not limited to) weak passwords that still meet complexity requirements, domain misconfigurations, overprivileged groups, and more. the idea is to showcase potential escalation & lateral movement throughout the domain.
<br />
<br />
The screenshots below will not cover every step of the process, but to cover the most important configurations and setups done prior to the demonstrated attack. For instance, images of the 'installation' process for both VMs on vmware will not be provided. 
