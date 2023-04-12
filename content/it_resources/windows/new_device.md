---
title: "New computer setup"
weight: 10
date: "2022-08-25"
author: "John Marks"
---

8/29/22

Installing and configuring Windows 10 for the University network (NO CM). 

## 1. Bootdisk
- F12 to get to the BIOS screen
    - Select the USB boot disk from the list of 'Boot Device' options
- In the Windows install screen
    - Select 'Custom: install Windows only'
    - Delete all existing drive partitions
        - If the application isn't seeing the disk drive it's likely that RAID setting in BIOS needs to be turned off
    - When all partitions are deleted select the 'Unallocated space' and then select 'new' and 'apply'
    - Wait for install process to complete, and then automatically restart
- After the restart work through the Windows prompts
    - When prompted for 'How would you like to set up?', select 'set up for an organization'
    - On the next screen, when prompted to 'sign in with Microsoft account', select 'domain join instead' at the bottom  
- When creating the initial account, make an 'admin' account
    - This 'local admin' account will be deprecated shortly, so this is a temporary measure
    - Do not opt-in to Cortana
    
## 2. Windows
- When Windows comes up, open the Edge browser and navigate to the Dell.com website and download the Support Assist tool from the Support section
    - When the install is completed, have it check for and run hardware updates
- Restart
- Run Windows updates
    - Restart

{{< hint warning >}}
**Offsite Users**\
If a user is entirely remote do not join the computer to the domain. Instead you need to manually install Office, Adobe, Chrome, Box, and the UNR VPN software directly off the web. Use Windows user management tools to create a user prior to sending the device out. Then email the user their needed credentials prior to sending the device out.
{{< /hint >}}

## 3. Adding device to Network/Domain
- Open 'Control Panel' and then 'System and Security' and then 'System'
    - Select 'Advanced system settings' from the sidebar
- Under the 'Computer Name' tab, select 'Change...'
- Set the computer name to CAS-$NAME
- Add the device as a Member of Domain and add the device to rd.unr.edu
- Restart

## 4. Initial login
- Login with NetId
- Check for the 'Software Center' app
    - If the software center app hasn't been installed yet, leave the computer connected to the UNR network to sync, restart after some time and check again
- Using the Software Center app install Office, Adobe Acrobat, and Box Sync
- From the web download and install Chrome
- Let software center run any updates it needs to
- Restart

## 5. Laptop devices
- In the Power Settings set:
    - When plugged in, turn off after - 3 hours
    - When plugged in, PC goes to sleep after - Never
- In Power Settings - Addtional power settings - Choose what closing the lid does, set:
    - When I close the lid, and the computer is plugged in - Do nothing