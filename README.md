# SOC-EDR-Homelab
## Configuring an Endpoint Detection & Response system for a Client Running Windows 11 (plus more!)

This project aims to simulate and analyze cyberattack scenarios in a controlled environment, using tools like Lima Charlie and Sliver to understand attack patterns, develop custom detection rules, and enhance incident response capabilities.

## First, for the simulation, I used a Windows 11 VM and an Ubuntu server VM with VMware workstation pro
📝This is a safe and controlled way to simulate real-world environments and cyber attack scenarios. Using virtual machines (VMs) ensures that any malicious activity is contained and doesn’t affect your actual operating system 📝
![Showing VMs]()

## For this project I used Lima Charlie as the EDR platform
📝Employing an Endpoint Detection and Response (EDR) tool like Lima Charlie is important. EDRs are crucial for detecting, investigating, and responding to cyber threats. 📝
![Showing LimaCharlie]()

## Using LimaCharlie I created and installed a sensor for the Windows 11 VM and also created an artifact collection rule
![Create and install sensor]()
![Create artifact collection rule]()

## I used SSH to connect to the Ubuntu server from my host machine, then downloaded Sliver for the C2 framework
📝Using SSH (Secure Shell) to manage the Ubuntu Server VM remotely is a secure method. It's also common practice in cybersecurity operations📝.
![Connect to Ubuntu Server with SSH]()
![Download sliver to Ubuntu]()

## In this part, I created a c2 payload and downloaded it to the Windows 11 VM
📝By deploying Sliver, a legitimate tool often used in adversary simulation and red teaming, I’ve added realism to the project. This step lets me understand how malware communicates to a command and control (C2) server. 📝
![Create c2 Payload with Sliver]()
![Now download to Windows 11]()

## Once I start the malware, I can see the connection being made in the C2 platform and I can see the EDR in the RCHP portion
![Start The Malware]()
![Notice RCHP]()

## We can take a look at our EDR platform and do the following actions.
-	Look for IOCs
![Look for IOCs]()
-	Search File hash with virus total
![Search file hash with virus total]()
-	Check the timeline
![Check timeline]()


## Going back to the Sliver, I check the privileges to see if I have “SeDeBugPriv”
![Check Privs]()

## In Sliver, I attempted to dump the process “lsass.exe,” and then I went to LimaCharlie to track down this process
📝Adversaries might dump the lsass.exe process to obtain user credentials and authentication tokens, enabling network infiltration, privilege escalation, and persistent access. 📝
![lsass accesses by malware]()

## In this environment, I decided I would like to be alerted if anything like this happened again, so I created a detection rule for this process
![build detection]()
![test detection]()
![save rule]()

## We can see that the rule works and will alert us if this happens again!
![rule working]()

# With LimaCharlie I took this a step further by automating a response in the EDR.
## First, I used my Sliver C2 framework to shell into the Windows 11 VM and then attempted to delete shadows.
📝Adversaries delete shadow copies to prevent data recovery after an attack, making it harder to restore encrypted or compromised files and increasing the efficacy of ransomware. 📝
![trying to delete shadows]()
## Next we can see in the images provided that the default SIGMA rules installed into the Limacharlie detected this; now I will use this event to build my own response rule and deploy it.
![default sigma rules picking up]()
![building a rule to block this action]()


In conclusion, the project successfully demonstrated the intricacies of cyberattack detection and response, highlighting the effectiveness of using advanced tools like Lima Charlie and Sliver in a simulated environment to develop and refine cybersecurity skills and strategies
