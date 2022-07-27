---
title: "Connecting to the UNR VPN"
description: ""
weight: 2
---

UNR has a VPN (Virtual Private Network) which allows for staff to access the university network from home or other 'remote' locations. Please note that this access is only available on request and is not granted implicitly.

*These instructions are an adaptation of: [UNR VPN Instructions](https://unr.teamdynamix.com/TDClient/2684/Portal/KB/ArticleDet?ID=117539)*

{{< hint danger >}}
**NOTE**\
Prior to completing these steps, open the File Explorer app in Windows and check under the 'This PC' section. If you can see any of the UNR Network drives; I:casat, or H:home you will need to 'disconnect' from them and restart your computer prior to connecting to the VPN. You can do this by clicking on the Network drives listed (should have a red 'x' on them) and then selecting 'disconnect'. **You must then restart your computer prior to connecting to the VPN or it will not work.**
{{< /hint >}}

## 1. Download and install GlobalProtect VPN App
- You can use this link to download the [GlobalProtect VPN App](https://border.unr.edu)
    - Use your NetId to login where promted
    - For our Windows computers select the '64 bit' Windows version
- Once the download is completed you can click on the file in your downloads folder and install the App
    - Just follow the installation and use any default options there is no need to change anything
    - If you are prompted with a security warning to verify the installation, select 'Yes'
        - Our users should have permission to complete this on their own devices, but if you are stuck here please contact me

## 2. Connecting to the UNR VPN
- Once the GlobalProtect App is installed you can find it by clicking on the ^ icon in the lower right corner of Windows
    - It should currently look like a grey circle (globe)
- Find the App and click on it, it should prompt you to input a 'Portal' address
    - If you do not see the correct menu, click on the lines in the upper right corner and go to 'Settings'
- Fill in the 'Portal' address and then press connect
    - Portal - border.unr.edu
- From there you should be prompted for a login for the VPN, fill in the login information and then press 'Connect'
    - Portal - border.unr.edu
    - Username - *Your NetId* (Not as an email, no @unr.edu)
    - Password - *Your NetId Password*

## 3. Navigating to the CASAT drive
- After connecting to the VPN you can navigate to the CASAT Network folder
    - You can check that you are connected to the VPN by mousing over the GlobalProtect VPN logo in the Windows task bar and hovering on it should show 'Connected', the globe will also be colored instead of black/white
- Open the File Explorer app in Windows and using the address bar(top center next to the Search) navigate to: 
    `\\storage.unr.edu`
    - If you are prompted for an additional login at this step, you need to write your NetId as an email this time *NetId*<i>*@*</i>*unr.edu*

{{< hint warning >}}
**Problems logging in**\
If your login is not being accepted on this window, you may need to double check the Username being used. You can do this by selecting 'More Choices' at the bottom of the login prompt. From there you can reinput your UNR NetId@unr.edu and Password.
{{< /hint >}}
