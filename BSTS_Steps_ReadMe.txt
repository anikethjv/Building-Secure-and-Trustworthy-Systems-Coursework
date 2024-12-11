Primary Requirements:
Operating systems in Virtual Environment - Kali Linux and Ubuntu

Steps:

1]Firstly we need to connect the two virtual machines, so that packets can be transferred between them.
  For this step, we configure the network connection to 'host-only' option, for both the virtual machines.

The IP addresses can be checked for both the virtual machines by running the command 'ifconfig'

IP address of Kali Linux VM - 192.168.136.128 
IP address of Ubuntu VM - 192.168.136.129 
  
2]To check the connection, we pinged the IP addresses from one VM to another, and verified that the virtual machines were connected to each other.

3]In target machine, we need to check for open ports, so that packets can be sent to that machine.
 using 'nmap 192.168.136.129', we got to know the open ports present in the target machine.
 after running the nmap scan, the following ports were open 
 port 80 - http
 port 22 - ssh  
____________________________________________________________________________________

SYN FLOOD ATTACK

4] From Metasploit Framework, we chose SYN Flood attack, which can be used to send SYN packets to target machine.

Configured the RHOST (target address/port), which is Ubuntu's IP address
and the SHOST (spoofable source address, to hide attacker's IP address)

=> set RHOST 192.168.136.129
=> set SHOST 192.168.136.200

5] After this configuration, we initiated the attack. To visualize the traffic, we started Wireshark, which analyzes all the incoming and outgoing traffic/packets.

6] To check the Memory and CPU Usage of target machine, after the attack initiated, we started System Monitor.

The stable graph of CPU and Network Usage sudden got spiked.

_____________________________________________________________________________________

hping3 Attack

We used the Flood option from hping3, to demonstrate the attack.

7] hping3 -S 192.168.136.129 -p 80 --flood 

-S is SYN flag, simulating the start of TCP connection
-p 80 means port 80 (HTTP)
--flood sends packets continuously, without waiting for responses

By running this command, we observed a request-response scenario.
But the amount of requests were very high as compared to amount of responses.

Again, a spike was observed in the target machine's Network usage and CPU usage.

______________________________________________________________________________________

hping3 Ping of Death attack

In this attack, large size of ICMP packets are sent to target machine. The size of these packets is usually higher than the maximum allowed packet size. Some systems are not designed to handle such large packets.

8]  hping3 -d 65430 -S -p 80 --flood 192.168.136.129

-d 65430 specifies the size of the packet. In this case, the size is 65430, which is more than enough to    overwhelm a target machine
-S is SYN flag
-p 80 means the port number
--flood sends packets continuously
and lastly the IP address is specified.

After executing this command, we observed results similar to normal hping3 attack, but the number of packets sent were enormous. The target machine's response time gets delayed because of this.

_______________________________________________________________________________________

Executing all the above mentioned attacks at once

In previous attacks, we attacked the target system, some delays in response time, and high CPU usage was observed. 
But the objective of DDoS attack is to make the target system unavailable to normal users, which wasn't possible using the above attacks.

So we initiated all the attacks at once, and observed the network traffic on Wireshark.
The packets sent to target machine were nearing 3 million, in no time. The Ubuntu VM was not responding to any clicks, and the System Monitor stopped responding.

After terminating all the attacks which were initiated, the target machine again started responding.