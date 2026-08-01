# Lab 01 - Basic VLAN Configuration

## What this lab is about

This is my first hands-on lab practicing VLAN segmentation on a Cisco switch. 
The goal was simple: split a network into two isolated groups using VLANs, 
and confirm they can't talk to each other without routing.

## Setup

I used one switch connected to four PCs. PC1 and PC2 belong to VLAN 10 (SALES), 
while PC3 and PC4 belong to VLAN 20 (TECH). Each PC got a static IP in its own 
subnet: 192.168.10.x for the SALES VLAN, and 192.168.20.x for TECH, both using 
a /24 mask.

## What I did

I started by creating the two VLANs on the switch and naming them SALES and 
TECH so they'd be easy to identify later. Then I assigned the switch ports to 
their matching VLAN using `interface range`, which let me configure two ports 
at once instead of repeating the same commands.

Once the VLANs were in place, I configured static IPs on each PC and tested 
connectivity. Ping between PC1 and PC2 worked fine since they're on the same 
VLAN, but pinging from PC1 to PC3 failed, exactly as expected since there's 
no routing between VLANs yet (that'll be Lab 02).

I also took the opportunity to secure the switch itself: an encrypted enable 
secret, a console password with login required, password encryption for 
anything stored in clear text, and a login banner warning unauthorized users. 
Passwords aren't included here on purpose, even though they're just test 
values — good habit to keep regardless of context.

## Commands I practiced

vlan, name, interface range, switchport mode access, switchport access vlan, 
enable secret, service password-encryption, line console 0, password, login, 
banner motd.

## Takeaway

VLAN isolation works as expected without any extra routing. Next step is 
adding inter-VLAN routing so SALES and TECH can communicate when needed, 
while everything else stays segmented.
