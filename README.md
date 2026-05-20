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











<sub>With the domain environment already established, the project continues by expanding domain management and user access. After initially accessing the client and domain controller with local credentials, Organizational Units are created for Employees and Administrators, followed by the creation of a dedicated Domain Admin account. The client virtual machine is then joined to the domain and organized within a Clients OU. Remote Desktop access is configured to allow standard domain users to sign in, and PowerShell automation is used to bulk-create user accounts within the Employees OU to simulate real-world provisioning tasks</sub>







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


<h2>2. JOIN VM CLIENT 1 TO DOMAIN<h2>

  
<sub>On your local machine, create another instance of Remote Desktop Connection, and Remote Deskop into "vm-client-1" using the public IP Address, and with your original admin user created in Azure but with domain credentials, mydomain.com\<yourcreatedcredentials> and password. Right-click the Windows start menu and select "System". Find and select "Rename this PC (advanced) on the right hand side > Select "Change" beside "To rename this computer or change its domain...". Under "Member of" select Domain, in the text field, type our created domain, "mydomain.com"</sup>

<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/b8ef06d9-850a-49a3-b72c-1bd182aaa2e6" />

Note: Because we configured client’s DNS to point to "vm-dc-1"," it can resolve the domain name and locate the Active Directory domain controller, which allows the domain login screen to appear.






Admin privileges are required to change this setting. Login using Jane Doe's username with domain credentials. Since Jane has admin rights, it will allow us to join the domain. You'll receive a pop-up that welcomes us to the domain. Click through the prompts, and close the system properties window, and select "Restart Now". And let Client 1 restart.

Back in our Domain Controller VM, reopen the Active Directory Users and Computers settings. Expand the domain tree and verify "vm-client-1" shows up in the Computers folder.





<img width="600" height="529" alt="image" src="https://github.com/user-attachments/assets/f096d960-753a-4a1e-afb0-9ba0553a6914" />


Right-click on our domain, and create a new OU "_CLIENTS". Drag "vm-client-1" from the Computers folder into the _CLIENTS folder.
<img width="500" height="525" alt="image" src="https://github.com/user-attachments/assets/db6a7fc7-de8b-4b96-9309-b535b52128b0" />


note: This OU structure illustrates best practices for directory organization by grouping administrators, employees, and client systems into separate management containers.


<h2>3. ENABLE REMOTE DESKTOP FOR USERS<h2>
  
Currently, users can sign in only when physically present at the computer; enabling Remote Desktop allows them to securely log in from another workstation and access their environment, which is a common practice in many organizations for flexibility and support.


Login to VM Client 1, with Jane Doe using domain credentials. Right-click the Windows start menu, and select "System". Find "Remote Desktop" on the right hand side. Under User accounts, select "Select users that can remotely..." > Select "Add..", in the Object names field, type "Domain Users" and select Check Names, once underlined, we can select OK.


<img width="800" height="729" alt="image" src="https://github.com/user-attachments/assets/4b338bd6-037a-4d88-a391-e7442b01f583" />
 Non-admin users can now log in. We will create those accounts in the next step. For now, sign out of "vm-client-1"







Note: “Domain Users” is a built-in security group in Active Directory that automatically includes all standard user accounts created    within the domain.


Note: Normally, Remote Desktop access is enabled through Group Policy, which allows administrators to configure this setting across multiple client machines at once.










<h2>4. CREATE EMPLOYEE USERS WITH AUTOMATION<h2>

We will automate the creation of multiple user accounts to simulate a real-world scenario where an IT administrator receives a list of new employees from HR. The script will generate these accounts in bulk while automatically placing them into the _EMPLOYEES OU.

Copy the folder "ad-script" from repository and place it on the desktop of "vm-dc-1". Open PowerShell ISE as Administrator, and open the AD PS Script file.


<img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/d2c1be29-f1e1-47bc-ae0a-2fd9354a2f77" />


Note: the line $PASSWORD_FOR_USERS   = "Password1" this indicates all accounts will be created with the same password. Run the script and observe the accounts being generated. When the script finishes, open Active Directory Users and Computers and verify the new users appear in the _EMPLOYEES OU.










<sub>Lastly, let's test the new accounts, choose any username and note the password from the script. Remote Desktop back into "vm-client-1" using the newly created credentials bab.vad  and confirm the account works successfully</sub>












<img width="957" height="477" alt="created employees" src="https://github.com/user-attachments/assets/727c1b7d-629a-472c-b9ad-6187240b83b0" />

















<img width="569" height="390" alt="emplyes user lock" src="https://github.com/user-attachments/assets/d6706150-ae57-4d3d-9b39-172aa310959b" />












<img width="1357" height="602" alt="last2" src="https://github.com/user-attachments/assets/b91e7799-00ed-4fc5-820c-789ba5b1bf73" />











<img width="850" height="477" alt="client 1 (1)" src="https://github.com/user-attachments/assets/dadb8e31-0e3b-47e9-b38e-80bb01949b69" />






