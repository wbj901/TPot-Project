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
