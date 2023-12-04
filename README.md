# SOC-EDR-Homelab
## Configuring an Endpoint Detection & Response system for a Client Running Windows 11 (plus more!)

This project aims to simulate and analyze cyberattack scenarios in a controlled environment, using tools like Lima Charlie and Sliver to understand attack patterns, develop custom detection rules, and enhance incident response capabilities.

## First, for the simulation, I used a Windows 11 VM and an Ubuntu server VM with VMware workstation pro
📝This is a safe and controlled way to simulate real-world environments and cyber attack scenarios. Using virtual machines (VMs) ensures that any malicious activity is contained and doesn’t affect your actual operating system 📝
![Showing VMs](https://github.com/Malikj10/SOC-EDR-homelab/blob/main/SOC-EDR-homelab-Screenshot/1This%20project%20needs%20a%20windows%20VM%20and%20a%20VM%20running%20ubuntu%20sever.%20Also%20for%20this%20lab%20we%20enable%20sysmon.png)

## For this project I used Lima Charlie as the EDR platform
📝Employing an Endpoint Detection and Response (EDR) tool like Lima Charlie is important. EDRs are crucial for detecting, investigating, and responding to cyber threats. 📝
![Showing LimaCharlie](https://github.com/Malikj10/SOC-EDR-homelab/blob/7156ace393e2cf0d67a2bc3175d116a3a8cc4b8c/SOC-EDR-homelab-Screenshot/2%20Use%20Lima%20charlie%20as%20EDR%20for%20this%20project.png)

## Using LimaCharlie I created and installed a sensor for the Windows 11 VM and also created an artifact collection rule
![Create and install sensor](https://github.com/Malikj10/SOC-EDR-homelab/blob/main/SOC-EDR-homelab-Screenshot/3%20Create%20and%20install%20sensor%20in%20windows.png)
![Create artifact collection rule](https://github.com/Malikj10/SOC-EDR-homelab/blob/7156ace393e2cf0d67a2bc3175d116a3a8cc4b8c/SOC-EDR-homelab-Screenshot/4%20Create%20artifact%20collection%20rule.png)

## I used SSH to connect to the Ubuntu server from my host machine, then downloaded Sliver for the C2 framework
📝Using SSH (Secure Shell) to manage the Ubuntu Server VM remotely is a secure method. It's also common practice in cybersecurity operations📝.
![Connect to Ubuntu Server with SSH](https://github.com/Malikj10/SOC-EDR-homelab/blob/7156ace393e2cf0d67a2bc3175d116a3a8cc4b8c/SOC-EDR-homelab-Screenshot/5%20Use%20SSH%20to%20connect%20to%20ubuntu%20server.png)
![Download sliver to Ubuntu](https://github.com/Malikj10/SOC-EDR-homelab/blob/7156ace393e2cf0d67a2bc3175d116a3a8cc4b8c/SOC-EDR-homelab-Screenshot/6%20download%20Sliver%20which%20is%20a%20c2%20framework%20and%20set%20that%20up.png)

## In this part, I created a c2 payload and downloaded it to the Windows 11 VM
📝By deploying Sliver, a legitimate tool often used in adversary simulation and red teaming, I’ve added realism to the project. This step lets me understand how malware communicates to a command and control (C2) server. 📝
![Create c2 Payload with Sliver](https://github.com/Malikj10/SOC-EDR-homelab/blob/83474a080a0b37cd71fb594a16ddc4be9668589d/SOC-EDR-homelab-Screenshot/7%20use%20sliver%20to%20create%20a%20c2%20payload.png)
![Now download to Windows 11](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/8%20make%20a%20temprary%20web%20server%20to%20get%20the%20payload%20from%20one%20computer%20to%20the%20next.png)

## Once I start the malware, I can see the connection being made in the C2 platform and I can see the EDR in the RCHP portion
![Start The Malware](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/9%20start%20malware%20in%20vm%20notice%20the%20connection.png)
![Notice RCHP](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/10%20notice%20RCHP%20as%20limacharlie%20and%20sliver%20in%20greeen.png)

## We can take a look at our EDR platform and do the following actions.
-	Look for IOCs
![Look for IOCs](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/11%20lets%20look%20at%20EDR.png)
-	Search File hash with virus total
![Search file hash with virus total](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/12.%20searched%20the%20hash%20with%20virus%20total%20but%20nothing%20is%20found!.png)
-	Check the timeline
![Check timeline](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/13%20scrolling%20back%20in%20the%20time%20line%20we%20can%20see%20where%20it%20is%20first%20on%20the%20system%20and%20all%20observe%20the%20network%20connections%20made.png)


## Going back to the Sliver, I check the privileges to see if I have “SeDeBugPriv”
![Check Privs](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/14%20SeDeBugPriv%20is%20powerful%20and%20we%20have%20it.png)

## In Sliver, I attempted to dump the process “lsass.exe,” and then I went to LimaCharlie to track down this process
📝Adversaries might dump the lsass.exe process to obtain user credentials and authentication tokens, enabling network infiltration, privilege escalation, and persistent access. 📝
![lsass accesses by malware](https://github.com/Malikj10/SOC-EDR-homelab/blob/ee5eaf0e4e2b6960480fd097d19aa0ddec2df8f7/SOC-EDR-homelab-Screenshot/15%20We%20can%20see%20lsass%20was%20accessed%20by%20our%20malware.png)

## In this environment, I decided I would like to be alerted if anything like this happened again, so I created a detection rule for this process
![build detection](https://github.com/Malikj10/SOC-EDR-homelab/blob/9776066c020a8761a70b89d13c2e5fbf83f54f96/SOC-EDR-homelab-Screenshot/16%20build%20detection%20rule%20for%20lsass%20acess.png)
![test detection](https://github.com/Malikj10/SOC-EDR-homelab/blob/9776066c020a8761a70b89d13c2e5fbf83f54f96/SOC-EDR-homelab-Screenshot/17%20testing%20rule%20against%20event%20.png)
![save rule](https://github.com/Malikj10/SOC-EDR-homelab/blob/9776066c020a8761a70b89d13c2e5fbf83f54f96/SOC-EDR-homelab-Screenshot/18%20save%20rule.png)

## We can see that the rule works and will alert us if this happens again!
![rule working](https://github.com/Malikj10/SOC-EDR-homelab/blob/9776066c020a8761a70b89d13c2e5fbf83f54f96/SOC-EDR-homelab-Screenshot/19%20detection%20rule%20working.png)

# With LimaCharlie I took this a step further by automating a response in the EDR.
## First, I used my Sliver C2 framework to shell into the Windows 11 VM and then attempted to delete shadows.
📝Adversaries delete shadow copies to prevent data recovery after an attack, making it harder to restore encrypted or compromised files and increasing the efficacy of ransomware. 📝
![trying to delete shadows](https://github.com/Malikj10/SOC-EDR-homelab/blob/9776066c020a8761a70b89d13c2e5fbf83f54f96/SOC-EDR-homelab-Screenshot/20%20shell%20into%20the%20system%20with%20malware%20and%20delete%20shadows.png)
## Next we can see in the images provided that the default SIGMA rules installed into the Limacharlie detected this; now I will use this event to build my own response rule and deploy it.
![default sigma rules picking up](https://github.com/Malikj10/SOC-EDR-homelab/blob/9776066c020a8761a70b89d13c2e5fbf83f54f96/SOC-EDR-homelab-Screenshot/21%20default%20sigma%20rules%20of%20EDR%20picked%20up%20on%20this.png)
![building a rule to block this action](https://github.com/Malikj10/SOC-EDR-homelab/blob/9776066c020a8761a70b89d13c2e5fbf83f54f96/SOC-EDR-homelab-Screenshot/22%20build%20our%20own%20rule%20to%20block%20this%20action.png)


In conclusion, the project successfully demonstrated the intricacies of cyberattack detection and response, highlighting the effectiveness of using advanced tools like Lima Charlie and Sliver in a simulated environment to develop and refine cybersecurity skills and strategies along with looking through telemetry.
