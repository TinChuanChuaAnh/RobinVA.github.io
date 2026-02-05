---
title: PcapPoisoning - picoCTF 2019  # Tên bài viết sẽ hiện to đùng
date: 2026-02-05 13:15:00 +0700      # Thời gian đăng (Quan trọng: +0700 là giờ VN)
categories: [CTF, picoCTF, Forensics]         # Danh mục lớn, danh mục con
tags: [wireshark, pcap, strings]     # Tag để tìm kiếm (viết thường)
author: "RobinVA"
---
# Write-up | picoCTF | PcapPoisoning
The link to the challenge is [here](https://play.picoctf.org/practice/challenge/362?category=4&page=2)
## Description
>*How about some hide and seek heh?*
>*Download this [file](https://artifacts.picoctf.net/c/374/trace.pcap) and find the flag.*
---
### Intial Analysis
I got a file named `trace.pcap`. After `file`-ed the file, we can confirm it's a real `.pcap` file. Now let's begin.

### Walkthrough
1. First, opening the file in Wireshark, we have an initial look at the traffic:
![Traffic](/assets/img/picoCTF/Forensics/PcapPoisoning/traffic.png)

Notice that there's FTP packet that contains the username and password:
![Sus packet](/assets/img/picoCTF/Forensics/PcapPoisoning/sus_packet.png)

Follow the packet via `Follow`->`TCP stream`, the filter is changed to `tcp.stream eq 1`, and we can see there's a TCP packet that appears:
![TCP](/assets/img/picoCTF/Forensics/PcapPoisoning/TCP.png)

We can clearly see the flag is right there. Copy it as ASCII text and we got the flag.

The flag is:
<details>
  <summary></summary>
  
  **picoCTF{P64P_4N4L7S1S_SU55355FUL_4624a8b6}**


</details>





