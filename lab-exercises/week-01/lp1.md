# Laboratory practice 1

## Task 1

Convert numbers.

| Hexadecimal | Decimal |
|-------------|---------|
| 20          |         |
| 01          |         |
| A9          |         |
| FF          |         |
| 14          |         |

|  Decimal    | Hexadecimal|
|-------------|---------|
| 144         |         |
| 12          |         |
| 254         |         |
| 56          |         |
| 158         |         |

| Binary      | Decimal |
|-------------|---------|
| 0101 0001   |         |
| 1110 1011   |         |
| 1010 0010   |         |
| 0001 0001   |         |
| 0010 0001   |         |

| Decimal     | Binary  |
|-------------|---------|
| 128  |         |
| 15   |         |
| 44   |         |
| 138  |         |
| 232  |         |

## Task 2

Open the command line interface on your PC. Experiment with the individual commands and their options, and record the function of each command.

### ipconfig (ifconfig for Linux/Mac, ip for Linux)
Displays all current TCP/IP network configuration values and refreshes Dynamic Host Configuration Protocol (DHCP) and Domain Name System (DNS) settings. Used without parameters, ipconfig displays Internet Protocol version 4 (IPv4) and IPv6 addresses, subnet mask, and default gateway for all adapters.

```powershell
ipconfig

ipconfig /all

ipconfig /displaydns

ipconfig /?

ipconfig /release

ipconfig /renew

ipconfig /flushdns
```

### arp
The arp command displays and modifies entries in the Address Resolution Protocol (ARP) cache. The ARP cache contains one or more tables that are used to store IP addresses and their resolved Ethernet or Token Ring physical addresses. There's a separate table for each Ethernet or Token Ring network adapter installed on your computer.

```powershell
arp

arp -a

arp -d *

```

### route
Displays and modifies the entries in the local IP routing table. If used without parameters, route displays help at the command prompt.

```powershell
route
```

### ping
Verifies IP-level connectivity to another TCP/IP computer by sending Internet Control Message Protocol (ICMP) echo Request messages. The receipt of the corresponding echo Reply messages is displayed, along with round-trip times. ping is the primary TCP/IP command used to troubleshoot connectivity, reachability, and name resolution. Used without parameters, this command displays Help content.

You can also use this command to test both the computer name and the IP address of the computer. If pinging the IP address is successful, but pinging the computer name isn't, you might have a name resolution problem. In this case, make sure the computer name you're specifying can be resolved through the local Hosts file, by using Domain Name System (DNS) queries, or through NetBIOS name resolution techniques.

```powershell
ping

ping stuba.sk

ping stuba.sk -t

ping stuba.sk -n

ping stuba.sk -l

ping stuba.sk -f

ping stuba.sk -f -l

ping stuba.sk -i 

ping stuba.sk -w
```

### tracert (traceroute for Linux/Mac, tracepath for Linux)
This diagnostic tool determines the path taken to a destination by sending Internet Control Message Protocol (ICMP) echo Request or ICMPv6 messages to the destination with incrementally increasing time to live (TTL) field values. Each router along the path is required to decrement the TTL in an IP packet by at least 1 before forwarding it. Effectively, the TTL is a maximum link counter. When the TTL on a packet reaches 0, the router is expected to return an ICMP time Exceeded message to the source computer.

```powershell
tracert

tracert -h

tracert -w
```

### netstat
Displays active TCP connections, ports on which the computer is listening, Ethernet statistics, the IP routing table, IPv4 statistics (for the IP, ICMP, TCP, and UDP protocols), and IPv6 statistics (for the IPv6, ICMPv6, TCP over IPv6, and UDP over IPv6 protocols). Used without parameters, this command displays active TCP connections.

```powershell
netstat -h

netstat -a

netstat -a -n

netstat -r

netstat -s

netstat -p <Protocol>
```

### nslookup
Displays information that you can use to diagnose Domain Name System (DNS) infrastructure.

```powershell
nslookup stuba.sk

nslookup stuba.sk 208.67.220.220
```

### dig (only for Linux/Mac)
This is a flexible tool for interrogating DNS name servers. It performs DNS lookups and displays the answers that are returned from the name server(s) that were queried. Most DNS administrators use dig to troubleshoot DNS problems because of its flexibility, ease of use, and clarity of output. Other lookup tools tend to have less functionality than dig.

```shell
dig stuba.sk

dig stuba.sk ANY

dig stuba.sk A

dig stuba.sk +short
```

### curl
curl (short for Client URL) is a command-line tool and library used to transfer data between your computer and a server.

```powershell
cd Downloads/

curl https://www.fiit.stuba.sk

curl -I  https://www.fiit.stuba.sk

curl -IL  https://www.fiit.stuba.sk

curl -ILO  https://www.fiit.stuba.sk/buxus/docs/Studium/studijne_odbory/informatika.pdf
```

### telnet
Communicates with a computer running the telnet server service. Running this command without any parameters, lets you enter the telnet context, as indicated by the telnet prompt (Microsoft telnet>). From the telnet prompt, you can use telnet commands to manage the computer running the telnet client.

```powershell
telnet telnet.example.com
```

### hostname
Displays the host name portion of the full computer name of the computer.

```powershell
hostname
```

## Task 3

1. Open file [lp1.pkt](./lp1.pkt) in PT.

2. Connect pc to router R1 port **gi0/0/0**.

3. Configure **PC0**:
    - Ipv4 Address: 192.168.0.2
    - Subnet Mask: 255.255.255.0
    - Default Gateway: 192.168.0.1
    - DNS Server: 2.2.2.2

4. Open **Command Prompt** on PC0.

5. Check ARP table:
    ```shell 
    arp  -a
    ```    
    Are there any entries visible in the ARP table?

6. Test connectivity to the gateway:
    ```shell
    ping 192.168.0.1
    ```

7. Check ARP table again:
   
   Are there any new entries?

   What does each entry look like, and what information does it show?

8. Ping DNS server:
    ```shell
    ping 2.2.2.2
    ```
    Was the ping successful?

    Are there any new entries in the ARP table?

9. Get the address of the server where our page **pks.net** is hosted:
    ```shell
    nslookup pks.net
    ```
    What are the DNS records for **pks.net**?

10. Trace the network path to **pks.net**:
    ```shell
    tracert pks.net
    ```
    How many hops are there between your machine and **pks.net**?

    What are the IP addresses of the intermediate routers?

11. Open your browser and type **pks.net**:

    Were you able to access the website successfully?
