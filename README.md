<p align="center">
<img width="1074" height="499" alt="image" src="https://github.com/user-attachments/assets/4c6a6c45-4b15-4273-9903-eec87961e8fa" />


This project builds on an existing Active Directory environment deployed in Microsoft Azure and focuses on core domain administration tasks, including user and group management, Organizational Unit (OU) structure, client domain joining, Remote Desktop access configuration, and automated user provisioning using PowerShell. The project demonstrates practical identity and access management workflows within a cloud-hosted Windows domain using industry-standard tools and configurations.



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>High-Level Deployment and Configuration Steps




<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/939d5997-9bc4-44eb-83f4-e7ed191ce001" />











With the domain environment already established, the project continues by expanding domain management and user access. After initially accessing the client and domain controller with local credentials, Organizational Units are created for Employees and Administrators, followed by the creation of a dedicated Domain Admin account. The client virtual machine is then joined to the domain and organized within a Clients OU. Remote Desktop access is configured to allow standard domain users to sign in, and PowerShell automation is used to bulk-create user accounts within the Employees OU to simulate real-world provisioning tasks.







- 1.Create Organizational Units for Employees and Admins
- 2.Create Domain Admin account
- 3.Join client VM to the domain
- 4.Configure Remote Desktop access for domain users
- 5.Automate bulk user creation with PowerShell
  

<h2>Installation Steps</h2>
1. CREATE A DOMAIN ADMIN USER.

On your local machine, Remote Deskop into "vm-dc-1" using the public IP Address, and with your original admin user created in Azure but with domain credentials, mydomain.com\<yourcreatedcredentials> and password. In the Windows search bar, find "Active Directory Users and Computers" (ADUC), alternatively you can go Start Menu > Windows Administrative Tools > Active Directory Users and Computers. Expand the “mydomain.com” domain. Right-click the domain and select New > Organizational Unit.
