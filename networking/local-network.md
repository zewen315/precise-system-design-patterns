# Local Network (Layer 2)

> The link layer — everything that happens before a packet ever needs a router.

## Ethernet & MAC Addresses

- What is a MAC address, and why is it burned into hardware rather than assigned hierarchically like an IP?
- What is an Ethernet frame, and what does it add on top of an IP packet (source/destination MAC, EtherType, FCS checksum)?
- Why do you need both a MAC and an IP address (MAC = "who are you on this wire", IP = "where are you on the internet")?

## ARP

- What problem does ARP (Address Resolution Protocol) solve — mapping a known IP address to the MAC address needed to actually put a frame on the wire?
- How does ARP work (broadcast "who has this IP", unicast reply), and what's the ARP cache for (`arp -n`, `ip neigh`)?
- What is ARP spoofing, and why is it possible (ARP has no built-in authentication)?
- What's the IPv6 equivalent of ARP (Neighbor Discovery Protocol, built on ICMPv6)?

## Switches

- What does a switch do differently from a hub (forwards a frame only out the port with the destination MAC, instead of broadcasting to every port)?
- What is a MAC address table, and how does a switch learn it (by observing source MACs on incoming frames)?
- What happens when a switch doesn't yet know which port a destination MAC is on (it floods the frame to all ports, like a hub, until it learns)?
- What is a broadcast domain, and why doesn't a switch break one up (only a router / L3 boundary stops a broadcast)?

## VLANs

- What problem does a VLAN solve — segmenting one physical switch into multiple isolated broadcast domains without separate hardware?
- What is 802.1Q tagging, and how does a trunk port carry multiple VLANs over a single physical link?
- Why does inter-VLAN traffic require a router (or L3 switch), even though the hosts sit on the same physical switch?

## Subnets, Gateway, DHCP

- What is a subnet, and why does it define the boundary of "who I can reach without going through a router"?
- What is the default gateway, and how does a host decide whether a destination is local or needs to go through it (comparing against its subnet mask)?
- What does DHCP hand out (IP address, subnet mask, gateway, DNS servers, lease time), and what's the DORA sequence (Discover, Offer, Request, Ack)?
- Why does an expiring DHCP lease rarely break an already-active TCP connection, but can break the next new one?

## MTU

- What is the MTU (maximum transmission unit), and why is Ethernet's default 1500 bytes?
- What happens when a packet exceeds the MTU of a link it needs to cross (fragmentation in IPv4, vs a "packet too big" ICMPv6 error with no fragmentation at all in IPv6)?
- What is Path MTU Discovery, and why can it silently break when a firewall drops the ICMP messages it depends on (the classic "black hole" MTU issue)?
- Why does an overlay network (VXLAN, a VPN, a container CNI) reduce the effective MTU, and what breaks if you don't account for it?

## Interview / SRE Usage

- A host can ping its gateway but nothing beyond it — what layer are you debugging, and what's the next check (routing table, then escalate up the stack)?
- Two hosts on the same subnet can't reach each other — what's your checklist (link state, ARP table, VLAN mismatch, switch port config)?
- Why would you see intermittent, size-dependent packet loss, and how does that point at an MTU/fragmentation problem?
