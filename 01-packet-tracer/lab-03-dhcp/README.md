# Lab 03 - DHCP Server Configuration

## What this lab is about

Building on Lab 02, I added a DHCP server to automatically assign IP addresses 
to PCs on VLAN 10, instead of configuring each one manually like in the 
previous labs. VLAN 20 was left on static addressing for comparison.

## Setup

Same topology as Lab 02, plus one server connected to the switch on an access 
port assigned to VLAN 10. The server itself keeps a static IP (192.168.10.10), 
since a DHCP server can't request an address from itself — it needs to be 
reachable at a known, fixed address for PCs to find it.

## What I did

I configured the switch port facing the server as an access port on VLAN 10, 
then gave the server a static IP. On the server, I set up a DHCP pool with a 
gateway of 192.168.10.1 and a starting address of 192.168.10.20, leaving some 
room below that for statically assigned devices like the router. Then I 
switched PC0 and PC1 from static to DHCP mode.

The first attempt didn't go as planned: PC0 received an IP, but with the wrong 
gateway (0.0.0.0 instead of 192.168.10.1). After comparing what I'd configured 
against what the PC actually received, I found the cause: Packet Tracer 
creates a default DHCP pool automatically the first time the service is 
enabled, and that default pool had incorrect values. It was answering the 
request before my own pool got the chance to.

The "Remove" button wouldn't delete that default pool (a known Packet Tracer 
quirk), so instead I edited its values directly to match my intended 
configuration. Once both pools had consistent values, the conflict was gone. 
Releasing and renewing the IP on PC0 confirmed it: correct address, correct 
gateway, correct DHCP server reference.

PC1 picked up the next address in the pool without any issue. VLAN 20 was left 
untouched on static addressing, and inter-VLAN routing from Lab 02 still works 
as expected, confirming the DHCP setup didn't break anything else on the 
network.

## Key commands and settings used

- `switchport mode access` / `switchport access vlan 10` (server's switch port)
- Static IP configuration on the server itself
- DHCP pool settings: default gateway, start IP address, subnet mask
- `ipconfig /release` and `ipconfig /renew` (to test and force new DHCP requests)

## Takeaway

DHCP now handles address assignment for VLAN 10, while VLAN 20 stays static — 
matching how real networks are usually managed: static addresses for servers 
and infrastructure, DHCP for regular end-user devices. The default pool 
conflict was a good reminder that tools can have their own hidden defaults 
that silently interfere with an intended configuration, and that comparing 
expected vs actual results is the fastest way to catch it.
