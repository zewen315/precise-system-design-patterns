# Network Tools

## `ip`

Modern replacement for `ifconfig`/`route`; manage interfaces, addresses, routes.

```bash
ip addr show          # list interfaces and IPs
ip route show           # routing table
ip link set eth0 up       # bring interface up
```

## `ss`

Socket statistics; modern replacement for `netstat`.

```bash
ss -tulpn         # listening TCP/UDP sockets with process info
ss -s               # summary of socket usage
ss dst <ip>            # filter by destination
```

## `tcpdump`

Capture and inspect packets on the wire.

```bash
tcpdump -i eth0 port 443         # capture traffic on a port
tcpdump -i any -w capture.pcap     # write capture to file for Wireshark
tcpdump -nn host 10.0.0.5            # filter by host, no DNS/service resolution
```

## `ping`

Test reachability and round-trip latency via ICMP echo.

```bash
ping -c 4 example.com     # 4 packets then stop
ping -i 0.2 example.com     # faster interval
```

## `traceroute`

Show the network path (hops) to a destination.

```bash
traceroute example.com
traceroute -T -p 443 example.com   # use TCP SYN instead of UDP/ICMP
```

## `mtr`

Combines `ping` + `traceroute` into a live, continuously updating view per hop.

```bash
mtr example.com
mtr -rw example.com     # report mode, wide output (for scripts/logs)
```

## `netcat` (`nc`)

Swiss-army knife for reading/writing raw TCP/UDP connections.

```bash
nc -l 8080                       # listen on a port
nc example.com 80                  # connect to a port
nc -zv example.com 20-30             # port scan a range
echo hello | nc -u host 514            # send UDP data
```

## `netstat`

Legacy tool for connections/routing/interface stats (superseded by `ss`/`ip`).

```bash
netstat -tulpn     # listening ports with PID/program
netstat -rn           # routing table
```

## `curl`

Transfer data to/from a URL; the default tool for probing HTTP(S) endpoints.

```bash
curl -v https://example.com                                      # verbose, shows request/response headers
curl -I https://example.com                                        # HEAD request only
curl -o out.json https://api.example.com/data
curl -w '%{time_total}\n' -o /dev/null -s https://example.com        # measure latency
```

## `dig`

Query DNS records with detailed output.

```bash
dig example.com              # A record
dig example.com MX             # specific record type
dig +short example.com           # just the answer
dig @8.8.8.8 example.com           # query a specific resolver
```

## `nslookup`

Older/simpler DNS lookup tool.

```bash
nslookup example.com
nslookup example.com 8.8.8.8   # query a specific server
```
