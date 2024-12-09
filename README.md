<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Configurations in Azure</h1>
This lab is a follow up to the lab where I installed Active Directory and created a domain controller. I will now be configuring Active Directory and allowing a client to join the domain as well as creating user accounts. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>Configuration Steps</h2>

![Screenshot 2024-12-06 150436](https://github.com/user-attachments/assets/0d4f26eb-f3ba-4a89-a04e-ec7c9df14e48)

![Screenshot 2024-12-06 150541](https://github.com/user-attachments/assets/7de7f731-48df-4df8-814b-62a7f8908f82)

Now that Active Directory is installed on the domain controller VM, it is time to create new Organizational Units and Users. With the Active Directory Users and Computers console open, right click on the domain you created (in my case, mydomain.com) and make a new Organizational Unit (OU). I have created two Organizational Units, _EMPLOYEES and _ADMINS. The reason behind this naming scheme is because a Powershell script will be utilized later. Within the _ADMINS OU, I created a new User called Jane Doe. Jane's account will be given administrative privileges through the use of a Security Group. To grant admin privileges to a User, right click on the user and open their Properties. Click Member Of then Add to apply the appropraite security group. In this case, I added Jane to the Domain Admins security group. From now on, I will be using Jane's account to make any further changes. I will be logging off as labuser and log in as jane_admin.

![Screenshot 2024-12-06 150757](https://github.com/user-attachments/assets/b1cc41c7-1b24-4851-a48a-5cf59e43d7e6)

Before the client can join the domain, it is important to configure the DNS settings first. The DNS server has to pointing to the domain controller's private IP address. On the Azure portal, open the Networking tab and click on Network Interface. In the DNS servers, enter the domain controller's private IP address and save the changes. Restart the client VM in order to ensure the DNS changes are saved. 

![Screenshot 2024-12-06 143532](https://github.com/user-attachments/assets/12703e98-909f-43f0-8f66-7ed485c67efc)

![Screenshot 2024-12-06 144056](https://github.com/user-attachments/assets/87457d2a-72a9-4365-a774-0b9a0d455db4)

It is now time to make the client VM join the domain. In the System menu of the client VM, click on Rename this PC (advanced) and Change. Enter the domain and necessary credentials in order to let the client join the domain. I am logging in as Jane Doe for the purposes of the lab. It is important to note that the login credentials have to be input within the context of the domain path. The client should now be part of the domain. On the domain controller, the client should now appear in Computers in the Active Directory Users and Computers panel.

![Screenshot 2024-12-06 151135](https://github.com/user-attachments/assets/98e83f65-d06a-43e4-bf6b-00b90d32bf95)

![Screenshot 2024-12-06 151248](https://github.com/user-attachments/assets/05b9f5d4-9202-44ab-adf0-0727b6c8fe24)

![Screenshot 2024-12-06 151434](https://github.com/user-attachments/assets/3a918bed-263d-4e65-bef3-9d6a6c5a700f)

Before users in the domain can use the client computer, Remote Desktop has to be enabled for non-administrative users. While logged in as the administrator (in my case, Jane), open System Properties. Click on Remote Desktop and Select users that can remotely access this PC. Allow Domain Users access to Remote Desktop. Non-administrative users can now log in to Client-1. Normally a Group Policy can do the same and allows changes to many systems at once. For the purposes of this lab, a Group Policy won't be used to make this change.

![Screenshot 2024-12-06 151947](https://github.com/user-attachments/assets/77018118-9b8a-407a-8a3a-0c5dded972cc)

Creating users can be done manually or through the use of a script. For this lab, I will be using a PowerShell script. The PowerShell script can be found <a href="https://github.com/AsiaPonder001/BunchofUsers/blob/main/README.md?plain=1)"> here. </a> On the domain controller, open PowerShell ISE as an administrator (and make sure you are logged in with an admin account on the domain controller). Create a new file and paste the script into ISE console. Run the script and observe the accounts being created. 

![Screenshot 2024-12-06 152539](https://github.com/user-attachments/assets/c0008155-6dc3-4e38-9c41-75235b721e1b)

![Screenshot 2024-12-06 152709](https://github.com/user-attachments/assets/51ff69ac-e0f1-4f95-9a5b-a15c150a2fa8)


After creating the users, Client-1 can now be signed in as one of the new users that were created from the PowerShell script. Pick a name and simply sign in to the client with the context of the domain. In my case, it is mydomain.com\bake.bih.

![Screenshot 2024-12-06 152817](https://github.com/user-attachments/assets/5e7d9d6a-bec7-4bb1-8dca-e2908fbd0892)

![Screenshot 2024-12-06 152853](https://github.com/user-attachments/assets/ae7da80e-8064-47e5-8bf8-4c6c7d82a118)

![Screenshot 2024-12-06 152908](https://github.com/user-attachments/assets/47a986fa-79d7-4afc-981e-7f04125e8d55)

<h2>Lessons Learned</h2>

Doing this lab has made me learn how to set up Active Directory and join clients to the domain. I also created users and assigned the necessary permissions. Active Directory is not difficult to learn despite all the menu navigation that takes place. This lab is a segway for me to learn about DNS settings in-depth and file permissions in action. I will go into detail about these topics in other labs.
