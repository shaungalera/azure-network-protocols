<p a href="center">

  ![image](https://github.com/user-attachments/assets/79937302-672e-4f02-9edf-07d98ff2e017)
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

<h2>Environments and Technologies Used</h2>

. Microsoft Azure (Virtual Machines/Compute)<br>
. Remote Desktop<br>
. Various Command-Line Tools<br>
. Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)<br>
. Wireshark (Protocol Analyzer)<br>

<h2>Operating Systems Used</h2>

. Windows 10 (21H2)<br>
. Ubuntu Server 22.04<br>

<h2>High-Level Steps</h2>

1.) Create Virtual Machines in Azure.<br>
2.) Observe ICMP traffic between Virtual Machines using Wireshark.<br>
3.) Configure a Firewall (Network Security Group) and analyze its impact on network traffic.<br>
4.) Observe various protocol traffic (SSH, DHCP, DNS, RDP) using Wireshark.<br>

<h1>Part 1: Create Virtual Machines</h1>

**1.) Log in to** [Azure Portal](https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize?redirect_uri=https%3A%2F%2Fportal.azure.com%2Fsignin%2Findex%2F&response_type=code%20id_token&scope=https%3A%2F%2Fmanagement.core.windows.net%2F%2Fuser_impersonation%20openid%20email%20profile&state=OpenIdConnect.AuthenticationProperties%3DsON0-jai790krMrk21_Iv00mcVrZMD9IluvQsz4ZaM_ME3UhhsSJDdicez-VakjcBZT6tvONzrOONCsEJ869d0aq4h3csgSc-aQucgurg4vKJ2fFjreFwH8Db9FSfKK_s0JDsksDw3hg6w4dVGq334ep-xpGRUQpH0HS-q1-8G_qw4aRHRDgEr7qNiuy50wiXWmO7wRp_ZkowxtAgF4eM2NqhhE_7cCshQE0KeDySHSfgqiCL0E9s4iLe1Vvx8_iiZQMebtqxAk8tuIRgoBPr98E-WMA8ypfJ1HrGhu64KInNuUD8_5QDWTVMA78ibHwHykZQV0gGGKGxPA5loeORTx9vyvFDwMhKOIdpOuoq2UJoayNdmm8iOQ83DmDcJFrLsAzQzyz6A7OO1HpyaLGuZaagOr5kb-MBQG6LHKNFoRmadMeHtBBjIeD59MNIX8kzrZ4auC6GELoCTROykYXu1jr5A9Eg2lc3I7RB5smBng1cY_p_CgWx4pYCONpvJNFpjuTPArXlpUWMYjGbJppYIR4Sk5teGFO5ecxFzw9mjZH3fPLshtVxtyZv19HxXvp&response_mode=form_post&nonce=638802648691811339.ZWJhMDIxZjItZDk4Mi00ZGQ2LTg0ZjAtN2RjYjFmMDk1MmNjNzY0OTI0NzctNzQxZS00ZmNkLThmMWYtNzU5ZGFjMGNlMTA1&client_id=c44b4083-3bb0-49c1-b47d-974e53cbdf3c&site_id=501430&client-request-id=efd57589-611a-40f4-90a8-546352cf44ff&x-client-SKU=ID_NET472&x-client-ver=8.3.0.0)<br>

**2.) Create a Resource Group**:<br />
. Navigate to "Resource Groups" and click "Create."<br>
. Provide a name for your Resource Group and select a region.<br>
. Click "Review + Create," then "Create."<br>

![image](https://github.com/user-attachments/assets/8e48c8a6-c773-48b2-b594-5a75a964b329)

**3.) Create a Windows 10 Virtual Machine**:<br>
. Navigate to "Virtual Machines" and click "Create."<br>
. Select the Resource Group you just created.<br>
. Configure the Virtual Machine:<br>
- OS: Windows 10<br>
- Create username and password<br>
- Head to the Networking section, then create a new virtual network titled "Lab2-vnet"<br>
. Complete the setup and deploy the VM.<br>

![image](https://github.com/user-attachments/assets/c7c9bb80-8054-4fe3-9a84-043fbad11892)
![image](https://github.com/user-attachments/assets/2a315d5d-48dc-4ff6-ba30-2aceff81cd75)
![image](https://github.com/user-attachments/assets/a56dc64d-5f24-4964-9abc-57b6bb3a8fbc)

**4.) Create a Linux (Ubuntu) Virtual Machine**:<br />
. Navigate to "Virtual Machines" and click "Create."<br>
. Select the same Resource Group and Virtual Network used for the Windows 10 VM.<br>
. Configure the Virtual Machine:<br>
- OS: Ubuntu Server 24.04<br>
- Authentication: Username/Password.<br>
. Ensure both VMs are in the same Virtual Network and Subnet as the Windows 10 VM.<br>
. Complete the setup and deploy the VM.<br>

![image](https://github.com/user-attachments/assets/53166f52-8704-40ab-83e0-0503f6fdf027)
![image](https://github.com/user-attachments/assets/db183cb0-5dd8-445d-92a2-2711fee0d94d)
![image](https://github.com/user-attachments/assets/7b0c398b-e479-491f-99f0-909b710570e9)
![image](https://github.com/user-attachments/assets/8b3adfaf-658a-4610-965f-76fce77b1797)

<h1>Part 2: Observe ICMP Traffic</h1>

**1.) Use Microsoft Remote Desktop to connect to your Windows 10 Virtual Machine (if on Mac, install the client first)**.<br />

**2.) Install Wireshark on the Windows 10 VM**:<br>
- Download and install [Wireshark](https://www.wireshark.org)<br />

**3.)Open Wireshark and start a packet capture.**<br>

**4.) Filter for ICMP traffic in Wireshark.**<br>

**5.)Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM**:<br>
- Open Command Prompt or PowerShell and run: *ping Ubuntu VM Private IP* .<br>
- Observe the ping requests and replies in Wireshark.<br>

**6.)From the Windows 10 VM, ping a public website (e.g., www.google.com) and observe the ICMP traffic in Wireshark.**<br>

![image](https://github.com/user-attachments/assets/66270e75-c391-4900-9be1-660c1e7ed915)
![image](https://github.com/user-attachments/assets/f8efdc28-dda6-471c-a338-d3d55c02dedd)

<h1>Part 3: Configure a Firewall (Network Security Group)</h1>
<h2>Observe ICMP Traffic with Firewall Changes</h2>

1.) Initiate a continuous ping from your Windows 10 VM to the Ubuntu VM:<br>
- Command: ping Ubuntu VM Private IP -t.<br>

2.) Open the Network Security Group associated with the Ubuntu VM.<br>
3.) Disable inbound ICMP traffic in the Network Security Group.<br>
4.) Observe the ICMP traffic in Wireshark and the command line Ping activity (should stop).<br>
5.) Re-enable ICMP traffic in the Network Security Group.<br>
6.) Observe the ICMP traffic in Wireshark and the command line Ping activity (should resume).<br>
7.) Stop the ping activity.<br />

![image](https://github.com/user-attachments/assets/515a4a68-b5ef-4d6b-bda7-41ce3ffdecec)
![image](https://github.com/user-attachments/assets/1a71309e-bcc3-4f4e-8169-17ba8dad5be9)
![image](https://github.com/user-attachments/assets/92419143-ce8a-4317-b650-2fef60039e32)
![image](https://github.com/user-attachments/assets/2913c84c-dc2f-45e9-a88b-5ed82c33e49b)
![image](https://github.com/user-attachments/assets/ccc4236d-791d-43b0-8461-1829fa40985a)

<h2>Observe SSH Traffic</h2>

1.) In Wireshark, start a new packet capture and filter for SSH traffic.<br>
2.) From the Windows 10 VM, SSH into the Ubuntu VM:<br>
- Command: ssh <username>@<Ubuntu VM Private IP.<br>
- Enter the password when prompted (the password will not be visible).<br>

3.) Type commands within the SSH session and observe the SSH traffic in Wireshark.<br>
4.) Exit the SSH session: exit.<br>

![image](https://github.com/user-attachments/assets/c3136ee4-321b-47ac-ac23-8274220aa9b8)
![image](https://github.com/user-attachments/assets/542f9f57-f45c-466b-8f96-80471e9bbd2f)
![image](https://github.com/user-attachments/assets/57b5d7bf-0af2-43f7-9a1b-21b635f60d47)

<h2>Observe DHCP Traffic</h2>

1.) In Wireshark, filter for DHCP traffic.<br>
2.) From the Windows 10 VM, issue a new IP address:<br>
- Open PowerShell as admin and run: ipconfig /renew.<br>

3.) Observe the DHCP traffic in Wireshark.<br>
4.)In this case our vm maintains the same IP. If we were to release our IP address (ipconfig /release) then renew it (ipconfig /renew) we would see the complete DHCP cycle in wireshark.<br>

![image](https://github.com/user-attachments/assets/3bee0a16-d24f-44bc-9fba-894df56ee381)

<h3>Observing the full DHCP Cycle</h3>
. Open notepad and type the release and renew commands<br>
. Choose a location to save the program. Here we chose c:\program data<br>
. You can name the file whatever you want but make sure to save it as a .bat file (this turns it into a simple script that we can run)<br>
. Make sure to change the 'save as type' to all files<br>

![image](https://github.com/user-attachments/assets/b1ab87f6-b3c1-4fcb-86c1-21f768c2f4c5)

. Change the directory that PowerShell is accessing to the location of the your .bat file by entering 'cd c:(filelocation)'<br>
. In this case I will change the directory to c:\programdata<br>
. Run the DHCP.bat script that was just created by entering '.\dhcp.bat'<br>
. This program should temporarily disconnect you from the vm because the IPv4 address is being released and renewed<br>
. Observe the Release - Discover - Offer - Request - Acknowledge steps in the DHCP process<br>
![image](https://github.com/user-attachments/assets/22ff9c74-576e-4efb-b88a-75075e98653e)


<h3>Observe DNS Traffic</h3>

1.) In Wireshark, filter for DNS traffic.<br>
2.) From the Windows 10 VM, use nslookup to find IP addresses for websites:<br>
- Example: nslookup disney.com.<br>

3.) Observe the DNS traffic in Wireshark.<br>

![image](https://github.com/user-attachments/assets/6f9ed88a-cab9-4e4c-b4bb-d99a5a67dfc0)

<h3>Observe RDP Traffic</h3>

1.) In Wireshark, filter for RDP traffic:<br>
- Use the filter: tcp.port == 3389.<br>

2.) Observe the continuous RDP traffic between the Windows 10 VM and your local machine.<br>

![image](https://github.com/user-attachments/assets/77e8c696-cbe1-48cc-a456-b2449cd77aab)
