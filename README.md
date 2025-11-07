<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Environments</h2>
Start by creating two virtual machines. Both VMs should be connected to the same virtual network (VNet).
The first virtual machine is a server that will serve as a Domain Controller, called DC-1. After creating the virtual machine, you need to make the IP address static. It could change if the IP is set to Dynamic. Go into the NIC card→ip config→ select static, then save. This will ensure the Client-1 pc will always be able to connect to the Domain Controller. Next, turn off the firewall on the Domain Controller. This is for testing purposes only. DO NOT do this on your personal PC.

The second virtual machine will be a basic desktop running Windows 11 Pro. Start by setting the Client-1 DNS settings to the DC-1 private IP address. Go into the NIC card→DNS servers→ Custom and enter the private IP of the DNS Server. Log in to Client-1 PC and ping the DNS server's private IP address. This will confirm everything is connected correctly. Running ipconfig /all will also verify the DNS Server IP address.


<h2>Preparing Active Directory Infrastructure in Microsoft Azure.</h2>

<p>
<img width="1911" height="1078" alt="2" src="https://github.com/user-attachments/assets/9c73b571-6f85-4dc6-9a6f-8336fb1216cb" />
</p>
<p>
Install and set up Active Directory. Log in to the DC-1 Server. Go to the Server Manager Dashboard→ Add role and features, select Active Directory Domain Services→add features and install.
</p>
<br />

<p>
<img width="1915" height="1077" alt="3" src="https://github.com/user-attachments/assets/9cae3bb8-40f0-4b35-a0d8-17adaf03b848" />
</p>
<p>
Promote DC-1 as a Domain Controller. Select the notifications button in the top right of the server manager dashboard; it will display a notification to promote this server to a Damian controller. Add a new Forest called Mydomain.com, create a password, and install. 
</p>
<br />

<p>
<img width="1919" height="1079" alt="4" src="https://github.com/user-attachments/assets/710dfa13-193e-4217-a0d4-15f79d25d03f" />
</p>
<p>
The next step is to create a Domain Admin user within the Domain. On DC-1, open Active Directory Users and Computers. Right-click mydomain.com→New → Organizational Unit named: “_EMPLOYESS”. Create a second Organizational Unit named: “_ADMINS”
</p>
<br />

<p>
<img width="1918" height="1079" alt="5" src="https://github.com/user-attachments/assets/981f4c1f-7d03-48d5-86e4-daba92233a88" />
</p>
<p>
Create a new user in the _ADMINS folder.
</p>
<br />

<p>
<img width="1913" height="1079" alt="6" src="https://github.com/user-attachments/assets/82873049-117a-4b2c-88ee-e49fd5890bd0" />
</p>
<p>
Name the user “Jane Doe” with the username of “jane_admin” and password: Cyberlab123!. Right-click Jane's account and go to properties. Select Member Of→Add→Enter the object names to select: Domain_Admins and select OK. Click Apply. This will make Jane a Domain Admin by adding her to the Domain Admins Security Group. This allows Jane to make changes to other users' accounts. Log out and back in to DC-1 as “mydomain.com/jane_admin”.
</p>
<br />

<p>
<img width="1919" height="1079" alt="7" src="https://github.com/user-attachments/assets/9b85634f-1855-4aae-98c4-cfc9fc994a3d" />
</p>
<p>
Now we will join Client-1 to the Domain (mydomain.com). Log in to Client-1 as labuser. Right-click Start menu→ System→Domain or Workgroup→Change→Member of, select Domain, enter mydomain.com, click OK. Log in as mydomain\jane_admin, password: Cyberlab123!. Restart Client-1.
</p>
<br />


<p>
<img width="1919" height="1079" alt="8" src="https://github.com/user-attachments/assets/52c1e09b-3e9c-4856-a1ad-aa740b75a690" />
</p>
<p>
Next, right-click mydomain.com and create a new Organizational Unit named:_CLIENTS. Then drag Client-1 into the newly created Organizational Unit named _ CLIENTS. Verify that Client-1 has joined the domain by logging in to the Domain Controller DC-1. Open Active Directory Users and Computers. Expand mydomain.com and select _CLIENTS to verify Client-1 is part of the Domain.
</p>
<br />

<p>
<img width="1918" height="1079" alt="9" src="https://github.com/user-attachments/assets/295bf739-3705-4c16-8f26-1dd14c87dabb" />
</p>
<p>
The next objective is to set up Remote Desktop for non-admin users on Client-1. Log in to Client-1 as mydomain.com\jane_admin. Right-click the Start menu and select system→Advanced System settings→Remote→Select Users→Add→Enter objective to select name: Domain Users and choose ok.
</p>
<br />

<p>
<img width="1918" height="1079" alt="10" src="https://github.com/user-attachments/assets/fced71a8-d07d-4934-a97d-8c556edfe430" />
</p>
<p>
Open Secure Shell and run a script that will create a bunch of users, then attempt to log in to Client-1 with a random user. Start by logging in to Domain Controller DC-1 as mydomain.com\jane_admin. Open PowerShell_ise as Admin. Create a new file and paste the Script's contents into it. Run the script to create thousands of random user accounts. It is now possible to log in to Client-1 with any of these random user accounts.
</p>
<br />
