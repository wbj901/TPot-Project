# Overview
This Azure honeypot project is a fake computer system/network that looks real but isn’t used for critical work. It’s designed to attract hackers who are up to no good. This project demonstrates skills in cloud infrastructure, network configuration, and security monitoring, while showcasing how attackers interact with exposed systems in the wild.

# What Is a Honeypot?
A honeypot is a deliberately vulnerable system designed to attract and study attackers. Security professionals use them to:

  - Observe attacker behavior and techniques.
  - Collect intelligence on malicious traffic.
  - Improve detection and defense strategies.

Think of it like a decoy house in a neighborhood: burglars break in thinking it’s an easy target, but instead, their activity is monitored and analyzed.

However, honeypots do have drawbacks:

  - They can be resource intensive.
  - Their scope is limited (they don’t replace full network defenses).
  - If not properly configured, they can become a launchpad for attacks on other systems.

# Steps
## Part 1 - Creating a Virtual Machine
The first thing you will do is go to the [Azure Portal](http://portal.azure.com/) and sign up for an account if you don’t already have one. Once you do, you will get $200 in free credits added to your account. That will cover more than the lab’s charges. Now search up virtual machines, click on the service, and you'll be brought to the screen shown below

![image](https://github.com/user-attachments/assets/46e86fb8-514b-4cb3-b0c3-1ec6dabe029c)

You should see a page similar to this:

![image](https://github.com/user-attachments/assets/4b362ba7-5319-40af-93a3-c0f2b63ba487)

### *1. Resource Group*
  - Click on <b>'Create New'</b> to create a resource group. You can name it whatever you like or you can call it `tpot-rg` for simplicity.
  - Note: A resource is the individual service that you will be consuming, and a resource group is a group of these resources together.
  - This project has a few resources such as the Virtual Machine, Public IP address, Network Security Group,… etc., inside this resource group. Once you're finished with the lab, just delete the resource group to delete this entire project.

### *2. Basics Tab:*
  - <b>Virtual Machine Name</b>: `tpot-vm` for simplicity or name it whatever you like 
  - <b>Region</b>: Choose a nearby region (I used <b>East US 2</b>).
  - <b>Availability Options</b>: Select <b>No infrastructure redundancy required</b>.
  - <b>Image</b>: Choose <b>Ubuntu Server 24.04 LTS</b>.
  - <b>Size</b>: Standard_B4ms (4 vCPUs, 16 GiB memory)
  - <b>Authentication</b>: Use <b>SSH public key</b> (recommended for security).
![image](https://github.com/user-attachments/assets/2fc61936-e2f5-4409-922a-79383fd94836)

### *3. Disks Configuration*
  - Set the OS disk size to 256 GiB.
(In earlier tests, smaller disks caused the :64297 web interface to fail to load.)
![image](https://github.com/user-attachments/assets/444678f3-0120-485f-97d2-8e43c35fce17)

### *4. Networking*
  - Leave most settings as default.
  - Check the option <b>“Delete public IP and NIC when VM is deleted.”</b>
(This ensures everything is cleaned up when you’re done)
![image](https://github.com/user-attachments/assets/0b09f6d5-379f-4c00-b283-52ca11d98182)


### *5. Review and Create* 
  - Click <b>Review + Create</b>.
  - Azure will validate your configuration.
  - Once validated, click <b>Create</b>.
  - <b>Download your SSH key</b> when prompted. You’ll need it later.

## Part 2 - Configuring the Network Security Group (NSG)
To allow global access (and attacks), open all inbound ports to the VM.

  - In the Azure search bar, search for your NSG (it’ll look like `tpot-vm-nsg`).
  - Go to <b>Settings</b> → <b>Inbound Security Rules</b> → <b>Add</b>.
  - Create a rule with the following values:
    - <b>Destination Port Ranges</b>: `*`
    - <b>Source Port Ranges</b>: `*`
    - <b>Priority</b>: `100`
    - <b>Name</b>: Choose a name or use `Allow-All` 
    - Click <b>Add</b> to apply the rule.
   ![image](https://github.com/user-attachments/assets/de7fc173-bb22-441f-908e-bec414ad9ce7)
⚠️ <b>Warning</b>: This configuration intentionally exposes your VM to the internet.
Do not reuse this environment for production or personal data.

## Part 3 - Connecting via SSH
If you’re using Windows, you’ll need PuTTY and PuTTYgen to connect via SSH.

  ### *1. Download & Install:*
  - [PuTTY](https://apps.microsoft.com/detail/xpfnzksklbp7rj?hl=en-US&gl=US)
  
  ### *2. Convert Your Key:*
  - In the Search bar, type and open <b>PuTTYgen</b> → <b>Click Conversions</b> → <b>Import Key</b>.
  - Select your downloaded `.pem` file.
  - Click <b>Save private key</b> and store it securely (this creates a `.ppk` file). 
![image](https://github.com/user-attachments/assets/634a19c8-20b7-40e7-a8b1-7b3cf2281a57)

### *3. Open PuTTY:*
  - Enter your VM’s <b>Public IP</b> in the Host Name field.
  - Go to <b>Connection</b> → <b>SSH</b> → <b>Auth</b> → <b>Credentials</b>.
  - Browse and select your `.ppk` private key.
  - Click <b>Open</b> to connect.

![image](https://github.com/user-attachments/assets/2228cc63-a6d5-40da-ac42-19a74887a225)


### *4. Login:*
  - Enter your username when prompted

## Part 4 - Installing T-Pot
Once connected to your VM terminal, run the following command to install T-Pot:
  - `env bash -c "$(curl -sL https://github.com/telekom-security/tpotce/raw/master/install.sh)"`
  - When prompted, select "<b>Hive</b>" or <b>"h"</b> installation type.
  - Create a username and password when prompted.
  - After installation, reboot the system:
  -   `sudo reboot`
    
Once the VM restarts, T-Pot services will automatically start.

## Part 5 - Accessing the T-Pot Web Interface 
After the reboot, open your web browser and visit:
  - `https://<Your_VM_Public_IP>:64297`
  - Log in using the credentials you created during installation.
You should now see the T-Pot landing dashboard showing various honeypot sensors and analytics.
![image](https://github.com/user-attachments/assets/846f0a70-ea3c-4ca1-8ee3-bb5083091f7e)

## *Summary*
Once your honeypot is live:
  - Let it run for a few hours to collect activity.
  - Explore dashboards to analyze:
    - IP addresses of attackers
    - Attack types and frequency
    - Common ports targeted
    - Malware samples (if any)
After you’re done collecting data, delete the resource group `tpot-rg` to clean up all associated resources and avoid extra Azure costs.
![imagge](https://github.com/user-attachments/assets/8389b2d4-fa49-41ee-a85d-06a41eaad189)
