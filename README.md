<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this home lab, I observed different networking activities and traffic using multiple ports such as ICMP, SSH, RDP, and DNS through the use of Wireshark and PowerShell on Windows and Linux VMs.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used</h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<h3>Part 1: Create Our Resources</h3>
1. Create a Resource Group.



2. Create a Windows 10 Virtual Machine (VM):

  ![Screenshot 2024-12-04 at 8 11 17 PM](https://github.com/user-attachments/assets/41570b0b-2389-401e-a0ab-312b9e57d3fa)
  
 ![Screenshot 2024-12-04 at 8 18 45 PM](https://github.com/user-attachments/assets/74dbf3c5-d92f-46f0-b2ec-bc6d020c672b)

4. Allow the creation of a new Virtual Network (Vnet) and Subnet.  
  
5. Create a Linux (Ubuntu) VM:  
  
    ![Screenshot 2024-12-04 at 8 16 59 PM](https://github.com/user-attachments/assets/e9bcc4f9-3f11-4094-8a67-5a7417643253)

7. While creating the VM, select the previously created Resource Group and Vnet. 
 
8. Observe your Virtual Network within Network Watcher.

<h3>Part 2: Observe ICMP Traffic</h3>
1. Use Remote Desktop to connect to your Windows 10 Virtual Machine.
  
   ![Screenshot 2024-12-04 at 8 20 05 PM](https://github.com/user-attachments/assets/73eecd57-6210-41fd-bb4e-edc24756580a)

2. Install Wireshark on the Windows 10 VM.
    ![Screenshot 2024-12-05 at 2 36 43 PM](https://github.com/user-attachments/assets/6987db48-60fe-497d-8148-0f7d0a8c7833)

3. Open Wireshark and filter for ICMP traffic only.  
  
   ![Screenshot 2024-12-05 at 3 23 51 PM](https://github.com/user-attachments/assets/09efed2d-9ab6-416c-be68-aa8e59134e0a)

  
4. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM.
   - Observe ping requests and replies within Wireshark.  
 
   ![image](https://github.com/user-attachments/assets/04d699d2-65f8-4bad-8d69-075962fd372d)
5. From the Windows 10 VM, open Command Line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in Wireshark.  
 
  ![Screenshot 2024-12-05 at 3 24 41 PM](https://github.com/user-attachments/assets/1eeb413f-9854-4f8b-ae7b-403ec0c86da6)

  
7. Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM.  

 ![Screenshot 2024-12-05 at 3 25 18 PM](https://github.com/user-attachments/assets/d77928fc-c4ca-48c9-850f-adae410a824b)

   - Open the Network Security Group (NSG) associated with your Ubuntu VM and disable incoming (inbound) ICMP traffic.  
  
 ![Screenshot 2024-12-05 at 3 28 24 PM](https://github.com/user-attachments/assets/eae8f638-0005-4308-888e-01a2bdf14ec3)

   
   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity.
   - Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using.
   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity (it should resume).
   - Stop the ping activity.

<h3>Part 3: Observe SSH Traffic</h3>
1. In Wireshark, filter for SSH traffic only.
2. From your Windows 10 VM, "SSH into" your Ubuntu VM (via its private IP address).
   - Type commands (username, password, etc.) into the Linux SSH connection and observe SSH traffic in Wireshark.
   - Exit the SSH connection by typing 'exit' and pressing [Enter].  
  
  ![Screenshot 2024-12-05 at 3 36 32 PM](https://github.com/user-attachments/assets/7c9e08c2-65b1-479c-ab38-90f770f13343)


<h3>Part 4: Observe DHCP Traffic</h3>
1. In Wireshark, filter for DHCP traffic only.
2. From your Windows 10 VM, attempt to issue your VM a new IP address using the command `ipconfig /renew` and observe the DHCP traffic in Wireshark.  

  ![Screenshot 2024-12-05 at 3 37 48 PM](https://github.com/user-attachments/assets/7accad64-d967-4851-bc01-c879792662ac)

   
<h3>Part 5: Observe DNS Traffic</h3>
1. In Wireshark, filter for DNS traffic only.
2. From your Windows 10 VM, within the Command Line, use `nslookup` to resolve a website's IP address and observe the DNS traffic in Wireshark.  
  
   ![Screenshot 2024-12-05 at 3 41 17 PM](https://github.com/user-attachments/assets/b5cae877-7598-4afc-8fb5-201fea08fc61)


<h3>Part 6: Observe RDP Traffic</h3>
1. In Wireshark, filter for RDP traffic only (`tcp.port == 3389`).
2. Observe the immediate non-stop traffic. Why is it non-stop versus only showing traffic when an activity occurs?
   
   RDP traffic is constant because it streams the live display of one computer to another. Unlike SSH, where traffic is generated only when keystrokes are sent, RDP transmits a constant stream of data for the remote session.  

   ![Screenshot 2024-12-05 at 3 42 29 PM](https://github.com/user-attachments/assets/8b7273ae-50e8-41a6-9c2b-204f0ee39de0)


<h2>Takeaways and Key Skills Developed</h2>
In this lab, I explored network security and traffic monitoring between Azure VMs using various protocols and tools such as Wireshark and PowerShell. I created both Windows and Ubuntu VMs, observing traffic for ICMP, SSH, DNS, RDP, and DHCP. By filtering traffic in Wireshark, I was able to understand how different protocols behave, such as viewing ICMP traffic during ping tests, SSH traffic during remote connections, and RDP traffic for remote desktop sessions. I also configured Network Security Groups (NSGs) to control traffic, such as blocking ICMP and monitoring its impact in real-time. This lab enhanced my understanding of network security practices, traffic analysis, and the configuration of security measures in a cloud environment.
