---
title: picoCTF - Sleuthkit Apprentice  # Tên bài viết sẽ hiện to đùng
date: 2026-02-06 20:10:00 +0700      # Thời gian đăng (Quan trọng: +0700 là giờ VN)
categories: [CTF, Forensics, picoCTF]         # Danh mục lớn, danh mục con
tags: [sleuthkit, disk image]     # Tag để tìm kiếm (viết thường)
author: "RobinVA"
---

# Write-up | BlueTeamLabs | Memory Analysis - Ransomware
The link to the challenge is [here](https://play.picoctf.org/practice/challenge/300?page=1&tag=27)
## Description
>*Download this disk image and find the flag.*

---

### Initial look

We are provided a `.img` file, which is a disk image. Basically, we will need to use Sleuthkit to solve this. Now let's begin.

### Walkthrough

**NOTE**: Please put all the outputs in `.txt` files for smooth analysis. The syntax is ```> <file_name>.txt```.

### 1. `mmls`

We will start with `mmls`. This command will show displays the contents of a volume system and where each partition starts and ends within that disk image <sup>[[1]](https://wiki.sleuthkit.org/index.php?title=Mmls)</sup> .

The command that we will execute is ```mmls disk.flag.img```

![mmls](/assets/img/picoCTF/Forensics/Sleuthkit_Apprentice/mmls.png)

To get more general details and information about all partitions, we can do `fsstat -o 2048 disk.flag.img`. 

We use `-o 2048` because the start of a physical disk is not the File System. We need to start from the partition where the OS is located, which is `2048`.

However, when I read some write-ups, it seems like the command does not help much, so I will skip it.

### 2. `fls`

`fls`, stands for "File listing", is a command used to list file and directory names in a disk image <sup>[[2]](https://tan-junwei.github.io/CTF-Writeups/PicoCTF/Forensics/Sleuthkit-Apprentice#mmls-command:~:text=The%20fls%20command%20lists%20file%20and%20directory%20names%20in%20a%20disk%20image.%20I%20used%20the%20%2Dr%20flag%20to%20search%20recursively%20for%20files%20and%20directories%20in%20each%20partition%20as%20well.)</sup>.

We will do ```fls -o 2048 disk.flag.img``` first:

![fls_1](/assets/img/picoCTF/Forensics/Sleuthkit_Apprentice/fls_1.png)

There's nothing special here, so we will take a look at another partition.

If you are going to analyse the ```Linux Swap / Solaris x86 (0x82)```, here is the thing: You can't.

![virtual_ram](/assets/img/picoCTF/Forensics/Sleuthkit_Apprentice/virtual_ram.png)

This partition is specifically used as Virtual RAM in case of RAM overload, so there's nothing such as Inode/MFT (do some research if needed) for `fls` to work with it. For that reason, we skip this paritition.

Our attention is now for the last partition:

```004:  000:002   0000360448   0000614399   0000253952   Linux (0x83)```

Do ```fls -o 360448 disk.flag.img```

![fls_3](/assets/img/picoCTF/Forensics/Sleuthkit_Apprentice/fls_3.png)

There are so much things here, so I try to `grep` the output.

The command is ```grep "flag" <your_fls3_output_file>.txt```

![grep_fls3](/assets/img/picoCTF/Forensics/Sleuthkit_Apprentice/grep_fls3.png)

Now we got two suspicious `.txt` files. Let's get the content of these two.

### 3. `icat`

`icat` is used to output the contents of a file based on its inode number ```icat -o <img-offset> <image-name> <inode-number>``` <sup>[[3]](https://tan-junwei.github.io/CTF-Writeups/PicoCTF/Forensics/Sleuthkit-Apprentice#mmls-command:~:text=icat%20is%20used%20to%20output%20the%20contents%20of%20a%20file%20based%20on%20its%20inode%20number%20icat%20%2Do%20%3Cimg%2Doffset%3E%20%3Cimage%2Dname%3E%20%3Cinode%2Dnumber%3E).

To get the result, we just need to do:

- ```icat -o 360448 disk.flag.img 2082```

and

- ```icat -o 360448 disk.flag.img 2371```

![result](/assets/img/picoCTF/Forensics/Sleuthkit_Apprentice/result.png)

The flag is ```picoCTF{by73_5urf3r_adac6cb4}```.


