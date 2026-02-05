# Write-up | picoCTF | PcapPoisoning
Link to the challenge: https://play.picoctf.org/practice/challenge/362?category=4&page=2
## Description
>*How about some hide and seek heh?*
>*Download this [file](https://artifacts.picoctf.net/c/374/trace.pcap) and find the flag.*
---
### Intial Analysis
I got a file named `trace.pcap`. After `file`-ed the file, we can confirm it's a real `.pcap` file. Now let's begin.

### Walkthrough
1. First, opening the file in Wireshark, we have an initial look at the traffic:
![Traffic](assets/img/traffic.png)

Notice that there's FTP packet that contains the username and password:
![Sus packet](assets/img/sus_packet.png)

Follow the packet via `Follow`->`TCP stream`, the filter is changed to `tcp.stream eq 1`, and we can see there's a TCP packet that appears:
![TCP](assets/img/TCP.png)

We can clearly see the flag is right there. Copy it as ASCII text and we got the flag.

The flag is <Click here>
  <summary>Click here</summary>
  
  **picoCTF{P64P_4N4L7S1S_SU55355FUL_4624a8b6}**


</Click here>
