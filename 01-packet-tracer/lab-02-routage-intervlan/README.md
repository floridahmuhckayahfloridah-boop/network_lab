# Lab 02 - Inter-VLAN Routing (Router-on-a-Stick)

## What this lab is about

Building on Lab 01, where VLAN 10 (SALES) and VLAN 20 (TECH) were completely isolated 
from each other, this lab connects them through a router using the router-on-a-stick 
method. The goal was to let devices on different VLANs communicate through a single 
physical link between the switch and the router.

## Setup

Same base topology as Lab 01 (1 switch, 4 PCs split across 2 VLANs), with a router 
added and connected to the switch through a single cable. Since only one physical link 
is used to carry traffic from both VLANs, the switch port facing the router had to be 
configured as a trunk instead of access, and the router needed subinterfaces to handle 
each VLAN separately.

## What I did

On the switch side, I configured the port connected to the router as a trunk port, 
allowing it to carry tagged traffic for both VLAN 10 and VLAN 20 over the same cable.

On the router side, I created two subinterfaces on the physical interface connected 
to the switch, one per VLAN. Each subinterface got a dot1Q encapsulation matching its 
VLAN number, plus an IP address that acts as the default gateway for that VLAN. I also 
had to remember to bring up the parent physical interface, since subinterfaces stay 
down otherwise even if properly configured.

Finally, I set a default gateway on each PC, pointing to the router's subinterface IP 
for their respective VLAN. I also had to adjust a couple of IP addresses, since the 
gateway needed the first usable address in each subnet, which was already taken by 
one of the PCs from Lab 01.

## Testing

Pinging from a VLAN 10 PC to a VLAN 20 PC failed on the very first attempt but 
succeeded on the next three. That's expected behavior — the first packet triggers ARP 
resolution between devices, so a short delay or a single timeout on the first ping is 
normal and not an issue.

## Commands I practiced

interface range, switchport mode trunk, interface (subinterface), encapsulation dot1Q, 
ip address, no shutdown, show ip interface brief, show interfaces trunk.

## Takeaway

VLANs isolate traffic by default, but a router can bridge them when controlled 
communication is needed, without giving up the segmentation itself. Next step would be 
adding access control (ACLs) to restrict which VLANs are allowed to talk to which, 
instead of allowing full open communication between them.
