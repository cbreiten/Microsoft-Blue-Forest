# Building a PFSENSE Firewall in Hyper-V

### Disclaimer: Everything I learned about PFSENSE is because of @da_667. If you want to learn more about PFSENSE and setting up virtual lab networks then buy his book here >> https://www.amazon.com/Building-Virtual-Machine-Labs-Hands/dp/1546932631/ref=sr_1_1?crid=T3NGL42B7MYL&keywords=building+virtual+machine+labs&qid=1558178883&s=gateway&sprefix=building+virtual+machi%2Caps%2C167&sr=8-1


1. In Hyper-V go to your switch manager. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE1.PNG)
   
2. You will need to create three virtual switches. 

- Create a virtual switch that can connect directly to your external network using the NIC card that provides your internet connectivity. 
   
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE2.PNG)
   
- Create a virtual switch for your internal network. Label it as your management network. Keep in mind the management network you will be able to access from your Hyper-V host.
   
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE3.PNG)
   
- Create a virtual switch for your private network. Lable it as your private switch. This switch will not have access to your host. This is so we can run things like Active Directory without affecting your host or other devices on your home network. 
  
  ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE4.PNG)
  
3. Create a New virtual machine on the actions menu

4. Specify the name and location of where you want to run your pfsense vm. I prefer to run mine on a secondary disk from where my host is running. 
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE5.PNG)
   
5. Select Gen 1 for the generation of the virtual machine type. 
 
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE6.PNG)

6. Assign 512mb of memory and uncheck use dynamic memory for this virtual machine. 
  
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE7.PNG)
   
7. For now select your bridged network connection for your connection type. We will add more connections after the VM is created. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE8.PNG)
   
8. Allocate 5 gig worth of space for the VM. This should be more than enough to run a pfsense box and upgrade the pfsense box as needed. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE9.PNG)
   
9. Select your pfsense iso. IF you don't have it you can download from this URL: https://www.pfsense.org/download/

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE10.PNG)
   
10. Click on finish.
   
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE11.PNG)
    
 ## Adding more network cards to your pfsense VM
 
 1. Double click on your pfsense VM and click on File >> Settings. Select Network Adapter under the hardware section. Click on Add.
    
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE12.PNG)
    
 2. Add your management network and your private switch.
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE13.PNG)
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE14.PNG)
    
 3. After all network cards are added. Power up the VM once. Once it is powered back on please shut it back down. This is so MAC addresses get allocated to all three network cards. 
 
 4. Open the VM settings under File >> Settings on your pfsense box again. Expand the "+" sign next to all three of your network adapters. Once they are all expanded you will need to set the mac address to static on each network card. Document the MAC address to your Bridged Network, Management Network, and IPS1 (Your Private switch). An example of this is shown below:
 
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE15.PNG)
   
5. Before we start our PFsense VM we should have the following documented:
   
   - MAC Addresses for each network card
   - The IP address range we want to use for the Management Network, and Private switch.
   - The IP address range for the private switch **must** match the IP address range your domain controller sits on

6. Below is what I have documented for my network layout and the mac addresses associated with each virtual switch.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE16.PNG)
   
7. Start the pfsense vm. It should autostart. Hit accept to the first screen.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE17.PNG)
   
8. Select OK to start installing pfsense.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE18.PNG)
   
9. Leave the default keymap and hit select to continue.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE19.PNG)
   
10. Select Auto UFS for the partition
  
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE20.PNG)
    
11. At this point the installed should start laying down the OS. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE25.PNG)
    
12. Select no if you are asked to open a shell at the end of install. This is not necessary. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE26.PNG)
    
13. Select reboot at the completion screen

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE27.PNG)
    
14. After the reboot it will be necessary to power off the vm. Once the VM is powered off remove the pfsense ISO from the virtual DVD drive. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE28.PNG)

15. Power back on the pfsense VM. Once it boots you will get asked if you want to set up VLAN's now. Type "n" for No to continue

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE29.PNG)
    
16. Next you will be asked to Enter the WAN interface. Type in hn0 to continue. You will notice the MAC address for hn0 matches the nic card that we set up for the bridged network.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE30.PNG)
    
17. For the LAN interface type in hn1. You will notice the MAC address for hn1 matches the nic card for the management network.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE31.PNG)
    
18. Finally for the optional interface type in hn2. You will notice the MAC address for hn2 matches the nic card for our private switch network card. Secondly you will notice it matches our IP address range I assigned to my domain controller.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE32.PNG) 
   
19. Type y to proceed with the configuration

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE33.PNG)
    
20. You will notice that the IP address for our LAN does not match what we actually need for our LAN (management network). This will need to be changed.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE35.PNG)
    
21. Enter Option 2 to assign IP addresses to our interfaces. Select 2 for LAN.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE36.PNG)
   
22. Enter your LAN IP address that you have selected for your management adapter. In this example my IP address is 172.16.1.1.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE37.PNG)
   
23. Enter 24 for the subnet mask range.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE38.PNG)
    
24. Press enter for the gateway upstream configuration. Nothing needs to be configured here.
    
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE39.PNG)
    
25. Press enter for the IPv6 configuration. Nothing needs to be configured here.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE40.PNG)
    
26. Press y to enable DHCP and press enter.
   
    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE41.PNG)
    
27. Type in your start and ending address range for your LAN1 configuration. I suggest leaving some finite amount of IP addresses for static addresses for your management network. In this example I am starting with 172.16.1.10 and ending it in 172.16.1.254. That leaves 172.16.1.2 - 172.16.1.9 as our static ranges which should be more than enough static ranges for the management network. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE42.PNG)
   
28. Select n for the http configurator question. We want to use https for our web configuration so no passwords can be intercepted when we use the web interface to configure the pfsense vm. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE43.PNG)
    
29. Take note of this URL. This is our pfsense web management url. Press enter to continue.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE44.PNG)

30. Select option 2 again so we can assign an IP address to OPT1 which would be our private network.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE45.PNG)
    
31. For the available interfaces select 3 to configure the OPT1 interface.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE47.PNG)
    
32. For the IPv4 address you will need to put your starting IP address for the IP range you select. In this example my range for the private network switch is 172.16.2.1 as shown below. Keep in mind this range needs to be used for the domain controller you may have already built. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE48.PNG)
    
33. The bit count should be set to 24 for the OPT1 network.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE49.PNG)
    
34. Press enter for the upstream gateway connection question. Nothing needs to be configured here. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE50.PNG)
    
35. Press enter for the IPv6 address. Nothing needs to be configured here. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE51.PNG)
    
36. Type y to enable the dhcp server option on OPT1.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE52.PNG)
    
37. Type in your start and ending address range for your OPT1 configuration. I suggest leaving some finite amount of IP addresses for static addresses for your management network. In this example I am starting with 172.16.2.10 and ending it in 172.16.2.254. That leaves 172.16.2.2 - 172.16.2.9 as our static ranges which should be more than enough static ranges for the private network. This is just a reminder that your active directory network sits on the OPT1 (Private network) connection so that it can have no contact with your hyper-v host or your other network devices sitting on your regular network. 

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE53.PNG)
    
38. Type n so that http is not reverted as the web configurator protocol. HTTPS should be used.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE54.PNG)
    
39. Press enter to continue.

    ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE55.PNG)
    
40. This concludes setting up the initial pfsense box through the shell interface. From this point forward you will be able to use the web interface to do the rest of the pfsense configuration.

## Hardening your defenses on your hyper-v host when using pfsense

Preface: Since we have a pfsense virtual machine up and part of it has the ability to communicate to our hyper-v host on the management network we will need to harden some of our defenses on our hyper-v host. Protecting your hyper-v host especially in a lab environment is important from a segregation perspective. 

1. From the Windows 10 host control panel and network connections you will need to right click on the vEthernet (Management Network) card. These network adapters get created any time you create a new virtual switch in hyper-v that is internally connected to your host. This is what gives your host the ability to communicate with anything on the hyper-v switch network connection. Select TCP/IPv4 properties. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE56.PNG)
   
2. Assign a static IP address to this network card. Since I used a IP address range of 172.16.1.1/24 and since my DHCP range started at 172.16.1.10, I will assign the IP address of 172.16.1.2 to my physical management network adapter. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE57.PNG)
   
3. Open the advance properties of the management network in the TCP/IPv4 configuration in the network card. Go over to the WINS tab. Please disable netbios over TCP/IP. It should not be needed and this setting with ensure attaching to things via SMB doesn't dump NTLM hashes when connecting to anything on your management network. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE58.PNG)
  
4. At this point we need to configure the Windows Defender Firewall to Deny connections on our hyper-v host coming from our management network. This further protects our hyper-v host from the management network. So if any inbound traffic happens to hit 172.16.1.2 if will get denied because of the firewall rule. If your management vnic has a different IP then adjust code accordingly. You can create the firewall rule in an elevated powershell window with the code below:

```
New-NetFirewallRule -DisplayName "Deny Inbound Traffic on Management vnic" -Direction Inbound -LocalAddress 172.16.1.2 -Action Block -Enabled True

```

## Configure pfsense VM through web interface

1. At this point open a web browser window. Google Chrome, Microsoft Edge or Firefox will be fine to use to access. 

2. The website address should the address of your HN1 (LAN) in pfsense. In this case my address is https://172.16.1.1. The Certificate errors can be ignored.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE60.PNG)

3. Sign into the website with the ID of "admin" and the password "pfsense"

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE61.PNG)
   
4. Click on Next to the first wizard screen.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE62.PNG)
   
5. Click on next again.
   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE63.PNG)
   
6. On the local domain and DNS setup click on next. Later I'll show how we can set DNS and the domain per network on LAN and OPT1. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE64.PNG)
   
7. Configure the time server according to the time zone you are in. I left the default NTP server but you are free to replace with a different NTP server if necessary. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE65.PNG)
   
8. The WAN network should be configured with DHCP. This is the default for this network. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE66.PNG)
   
9. Uncheck "Block RFC1918 private networks entering via WAN. This is left uncheck because in my configuration the WAN network is using a RFC1918 private network for the WAN address. This only needs to be checked if your WAN network is using a public IP outside of a private address space. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE67.PNG)
   
10. The LAN address was initially set up during the PFSENSE installation process. I don't wish to change my LAN address so I will click on next for this setting. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE68.PNG)
   
11. At this screen you are required to change the password to a secure password of your choosing. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE69.PNG)
   
12. We are now towards the end of the pfsense setup process. Click on the reload button. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE70.PNG)
   
13. You will now see screen with the reload of pfsense in progress. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE71.PNG)
   
14. Click on Finish now that pfsense is configured. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE72.PNG)
   
15. Click on accept to the banner about pfsense not allowed for commercial distribution

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE73.PNG)

16. On the top menu click on Firewall > Rules. Make sure LAN is selected. Pay attention to the lockout rule at the top of the menu. We will set up a new anti lockout rule that is specifically pointed to your machine host. We will also force it over an encrypted connection on port 443. Click on the "Add button" with the up arrow.

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE74.PNG)
   
17. Next you will want to allow traffic over TCP/IPv4 to your host IP that is 172.16.1.2. This is the IP you assigned to your adapter to communicate to your management (LAN) Network. The destination address will be over https (443) to the IP of your pfsense vm. For mine it is 172.16.1.1. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE75.PNG)
   
18. I've called this rule a hardened antilockout rule. Click on Save when finished. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE76.PNG)
   
19. Click on apply to put the changes into effect. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE77.PNG)
   
20. Next you should see a message that the change was applied successfully. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE78.PNG)
   
21. Now we will need to delete the default antilockout rule as shown below. Click on the gear icon to the right of the rule. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE79.PNG)
   
22. Place a checkmark next to the setting to disable the webconfigurator anti lockout rule. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE80.PNG)

23. At this point you can save the changes and it should be successfully applied. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE81.PNG)
   
24. Your Rules should look similiar to this. As you will notice earlier in my documentation 172.16.1.2 is the static IP I gave my management vnic. This rule ensures only my management vnic has access to my pfsense web interface. 

   ![](https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/Screenshots/PFSENSE82.PNG)
   
25. At this point your LAN network is configured. Keep in mind this is a very basic firewall setup that is any traffic over IPv4 and IPv6. For the OPT1 network you will need to add the same IPv4 and IPv6 fules as the LAN network. The reason why I am allowing all traffic is that we will configure the Windows Firewall through group policy to restrict traffic within our OPT network. 

26. Your OPT1 network rules should look like this:
