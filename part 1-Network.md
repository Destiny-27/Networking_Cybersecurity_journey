Week 1: Network Discovery
 
## What I Did
This week I explored how my own laptop connects to a network. I used Command Prompt on Windows to look at my network settings and run a few basic diagnostic commands. I was connected through my phone's Wi-Fi hotspot.
 
---
 
## My Network Info
 
| Property | Value |
|---|---|
| My IP Address | 172.20.10.2 |
| Subnet Mask | 255.255.255.240 |
| Default Gateway (router) | 172.20.10.1 |
| My MAC Address | C8-B2-9B-82-F5-87 |
 
The **IP address** is like my laptop's temporary "street address" on this network. The **MAC address** is more like a serial number — it's built into my laptop's Wi-Fi hardware and doesn't change. The **gateway** is my phone, since it's acting as the router.
 
---
 
## Commands I Ran
 
### `ipconfig /all`
Shows all the network info for my laptop — my IP address, subnet mask, gateway, and MAC address. This is where I found the info in the table above.
 
### `ping 172.20.10.1`
Sends a few small test messages to my router (my phone) to check if I can actually reach it. All 4 came back successfully, which means my connection is working.
 
### `arp -a`
Shows which devices my laptop has recently talked to on the network, and matches each one's IP address to its MAC address. On my network, the only real device (besides my own laptop) was my phone, at 172.20.10.1. The other entries in the list are just reserved addresses the network uses automatically, not actual devices.
 
### `tracert google.com`
Shows the path my data takes to reach Google, hop by hop, through different routers. It took 10 hops to get there. A few steps along the way didn't respond (shown as "Request timed out"), which I learned is normal — some routers just don't reply to this type of request.
 
### `nslookup google.com`
Looks up the actual IP address behind "google.com." Turns out google.com is really the address 142.251.216.110. This helped me understand that every website name is just a friendlier way of pointing to a number.
 
---
 
## What I Learned
Running these commands helped me actually see, instead of just read about, how a device joins a network: it gets an IP address automatically, it can find other devices using MAC addresses, and it can reach websites by looking up their real IP address first. Small steps, but it's starting to make sense.
