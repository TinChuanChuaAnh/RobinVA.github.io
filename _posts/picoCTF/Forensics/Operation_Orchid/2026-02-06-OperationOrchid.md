---
title: picoCTF - Operation Orchid  # Tên bài viết sẽ hiện to đùng
date: 2026-02-06 21:22:00 +0700      # Thời gian đăng (Quan trọng: +0700 là giờ VN)
categories: [CTF, Forensics, picoCTF]         # Danh mục lớn, danh mục con
tags: [sleuthkit, disk image, openssl]     # Tag để tìm kiếm (viết thường)
author: "RobinVA"
---

# Write-up | picoCTF | Operation Orchid
The link to the challenge is [here](https://play.picoctf.org/practice/challenge/285?page=1&tag=27)
## Description
>*Download this disk image and find the flag.*

---

### Walkthrough

We also get a `.img` file like the previous challenge: Sleuthkit Apprentice. In case you haven't read my write-up for that challenge, click [here](https://robinva-uit.github.io/posts/SleuthkitApprentice/). You can follow the same process that I did in the last write-up, so I will skip those steps.

We will begin at the point after we did `grep` to find files that have the keyword "flag":

![fls_4](/assets/img/picoCTF/Forensics/Operation_Orchid/fls_4.png)

You can try `icat` the first file, but it won't show anything valuable. Our focus is now on the second file, which has the extension `.enc`.

After a quick search, I know this file is encrypted and need some kinds of decryption to get the info. Still, we haven't known what kind of encryption was done onto the file yet.

This leads me to explore the `.ash_history` file in the partition.

If you do not know what the file is used for, it works like a log: records all of commands that you executed on the Terminal.

![ash_history_location](/assets/img/picoCTF/Forensics/Operation_Orchid/ash_history_location.png)

![offset](/assets/img/picoCTF/Forensics/Operation_Orchid/offset.png)

The `inode-number` of `.ash_history` is `1875`, and the starting offset of the partition is `411648`. Now we do `icat` with it:

```icat -o 411648 disk.flag.img 1875 > ash_history.txt```

![icat](/assets/img/picoCTF/Forensics/Operation_Orchid/icat.png)

As you can see, the hacker used `aes256` algorithm to encrypt the flag. Knowing this, we do:

```icat -o 411648 disk.flag.img 1782 > enc```
to get the encrypted file out, and:

```openssl aes256 -d -salt -in enc -out flag.txt -k unbreakablepassword1234567```
to get the flag.

![result](/assets/img/picoCTF/Forensics/Operation_Orchid/result.png)

The flag is `picoCTF{h4un71ng_p457_5113beab}`


