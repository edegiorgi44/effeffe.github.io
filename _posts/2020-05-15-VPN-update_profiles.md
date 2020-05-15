---
layout: post
title: Import and update OpenVPN profiles to NetworkManager
description: Linux
---
# Ovpn profiles import to NetworkManager
You have just subscribed to a new VPN company which provides access via OpenVPN.  
That's wonderful, but there is no app available for Linux to manage all the locations available, or you just don't trust that App. What to do? There should be a way to automatise the import to the system, right?  

Indeed, it is possible to make a script to do this, which I wrote and uploaded here [1](https://github.com/effeffe/Misc/blob/master/VPN-update_profiles.sh).  
First, download it to the folder where you OpenVPN profiles are, then set the username and the password that you use to log in as follows:
```
6 # DEFINE your username and password here:
7 USERNAME=username_here
8 PASSWORD=password_here
9 #
```
apply now execution permissions to the screipt as `chmod +x VPN-update_profiles.sh` and execute it. Please note that, according to your system configuration, the script could require root permissions to modify NetworkManager connections.  

To explain what the script does in a few words, the script updates your VPN configuration for the corresponding profiles present in the current folder you are working in.  
In more words, it removes all the openvpn connections in your system that correspond to the profiles in the current folder, then adds all the .ovpn profiles that are in the folder and then sets the username and the password declared at the beginning of ther script.  
In case your login system does require other parameters, you may want to modify the script accordingly.

## References
[1] : [https://github.com/effeffe/Misc/blob/master/VPN-update_profiles.sh](https://github.com/effeffe/Misc/blob/master/VPN-update_profiles.sh)
