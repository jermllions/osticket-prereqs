<p align="center">
<img width="1074" height="499" alt="image" src="https://github.com/user-attachments/assets/4c6a6c45-4b15-4273-9903-eec87961e8fa" />


<small>This project builds on an existing Active Directory environment deployed in Microsoft Azure and focuses on core domain administration tasks, including user and group management, Organizational Unit (OU) structure, client domain joining, Remote Desktop access configuration, and automated user provisioning using PowerShell. The project demonstrates practical identity and access management workflows within a cloud-hosted Windows domain using industry-standard tools and configurations<small>



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>High-Level Deployment and Configuration Steps




<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/939d5997-9bc4-44eb-83f4-e7ed191ce001" />











<small>With the domain environment already established, the project continues by expanding domain management and user access. After initially accessing the client and domain controller with local credentials, Organizational Units are created for Employees and Administrators, followed by the creation of a dedicated Domain Admin account. The client virtual machine is then joined to the domain and organized within a Clients OU. Remote Desktop access is configured to allow standard domain users to sign in, and PowerShell automation is used to bulk-create user accounts within the Employees OU to simulate real-world provisioning tasks<small>







- 1.Create Organizational Units for Employees and Admins
- 2.Create Domain Admin account
- 3.Join client VM to the domain
- 4.Configure Remote Desktop access for domain users
- 5.Automate bulk user creation with PowerShell
  

<h2>Installation Steps</h2>
1. CREATE A DOMAIN ADMIN USER.

On your local machine, Remote Deskop into "vm-dc-1" using the public IP Address, and with your original admin user created in Azure but with domain credentials, mydomain.com\<yourcreatedcredentials> and password. In the Windows search bar, find "Active Directory Users and Computers" (ADUC), alternatively you can go Start Menu > Windows Administrative Tools > Active Directory Users and Computers. Expand the “mydomain.com” domain. Right-click the domain and select New > Organizational Unit.





<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/11fb0c7d-2f8d-4b43-ac3a-a2d3c3a9f09f" />








In the Name field, type “_EMPLOYEES”. An underscore is used to differentiate this Organizational Unit from the default Windows containers within the domain tree. Repeat steps to create a new OU for "_ADMINS". Right-click our Admins folder and select New > User. Create user Jane Doe, with a username "jane_admin" and select Next, in the Password screen, unmark the checkbox for "User must change password at next logon", and mark "Password never expires check box", and choose a password.




For the purposes of this lab, a simplified password policy is used to streamline setup and testing; however, these settings would not be recommended in a production environment.




<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/ff80f495-dc79-498d-b964-4fa7b5d27f6f" />




Even though Jane Doe was created in our _ADMINS folder, it does not automatically designate her as admin, we will need to add her to the built-in Domain Admins Security Group. Right-click user Jane Doe, find tab "Member of" and select "Add..", in the Object names field, type "Domain Admins" and select Check Names, once underlined, we can select OK. Keep the Remote Connection on.




<img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/bf2ec6cf-75e1-45a4-8a21-f9de984f663e" />

Our Jane Doe is now an Admin, log out of the "vm-dc-1" and log back in with Jane Doe's username, using domain credentials, mydomain.com\jane_admin and password. We will continue using Jane Doe from now on.




