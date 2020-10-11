---
title: "Write Up Diskoctf"
date: 2020-10-10T23:23:47+07:00
draft: false
author: fallcrescent
cover: /images/diskoctf/logokominfo.png
---
## Misc - Bonus
Untuk soal kategori ini tidak perlu di jelaskan sudah ada jawabanya semua
![bonus](/images/diskoctf/bonus.png)
flag nya yaitu\
Bonus 1 : **DiskoCTF{reborn_playing}**\
Bonus 2 : **DiskoCTF{hints!}**\
Bonus 3 : **DiskoCTF{we've_should_sorry_to_you_guys}**  

## Reverse Engineering
### Expr or Call
Berikut adalah challengenya

>Adakah jalan (commands) menuju bendera diantara alat dynamic debuggers ?  P.S: Akan sangat disayangkan bila soal ini di-solve menggunakan Decompiler , such waste time :)

buka menggunakan gdb lalu pasang breakpoint pada fungsi main
```bash
gdb -q main
break main
```
jalankan kembali programnya dengan menggunkaan perintah r pada gdb, selanjutnya masukan perintah jump pada fungsi flag, dan di dapatkan flagnya
```bash
gdb-peda$ jump flag
Continuing at 0x555555555154.
DiskoCTF{when_there's_symbols_why_should_disassemble_it?}
[Inferior 1 (process 9099) exited normally]
```

**Flag: DiskoCTF{when_there's_symbols_why_should_disassemble_it?}**


## OSINT
Kali ini challenge nya tentang OSINT (Opensource Intelligence) itu arti nya skill Intelligence Gathering kita bakalan dipraktikkan disini.
### Rahasia Username
>Ada rahasia dibalik username diskominfokotaserang

pertama kita menggunakan tools [sherlock](https://github.com/sherlock-project/sherlock) untuk mencari tau kumpulan akun sosial media dengan menggunakan tools itu dan di dapatkan hasil sebagai berikut
```bash
[fallcrescent@ccug ~]$ sherlock diskominfokotaserang
[*] Checking username diskominfokotaserang on:
[+] Academia.edu: https://independent.academia.edu/diskominfokotaserang
[+] Blogger: https://diskominfokotaserang.blogspot.com
[+] CashMe: https://cash.me/$diskominfokotaserang
[+] Disqus: https://disqus.com/diskominfokotaserang
[+] GitHub: https://www.github.com/diskominfokotaserang
[+] Instagram: https://www.instagram.com/diskominfokotaserang
[+] Photobucket: https://photobucket.com/user/diskominfokotaserang/library
[+] Pinterest: https://www.pinterest.com/diskominfokotaserang/
[+] allmylinks: https://allmylinks.com/diskominfokotaserang
```
 ada sesuatu yang menarik pada postingan instagram diskominfokotaserang yaitu ![instagram](/images/diskoctf/rahasia-username.png)  setelah di download buka dengan menggunakan stegsolve dan menggeser beberapa bit mendapat penampakan flagnya
![flag rahasia username](/images/diskoctf/flag-rahasia-username.png)  
**Flag: DiskoCTF{coloring_osint}**\
### OSEEN
>Melihat sebuah komentar di platform "lebih dari tv" mungkin akan mendapatkan flag

kita di beri gambar sebagai berikut:  
![oseen](/images/diskoctf/340.png)
kita periksa meta datanya dengan menggunakan exiftool dan di dapatkan informasi sebagai berikut
```bash
[fallcrescent@ccug Downloads]$ exiftool 340.png 
ExifTool Version Number         : 12.00
File Name                       : 340.png
Directory                       : .
File Size                       : 2.1 MB
File Modification Date/Time     : 2020:10:11 14:57:26+07:00
File Access Date/Time           : 2020:10:11 14:57:26+07:00
File Inode Change Date/Time     : 2020:10:11 14:57:33+07:00
File Permissions                : rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 1920
Image Height                    : 1080
Bit Depth                       : 8
Color Type                      : RGB
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Significant Bits                : 8 8 8
Exif Byte Order                 : Little-endian (Intel, II)
Software                        : Google
Artist                          :  supercell
Copyright                       : Sony Music Entertainment (Japan) Inc. (on behalf of (P)2009 Sony Music
Exif Version                    : 0220
User Comment                    : 「君の知らない物語」（Cover）-波羅ノ鬼（ハラノオニ）-
Image Size                      : 1920x1080
Megapixels                      : 2.1
```
Pada user Comment terdapat lagu yang di bawakan oleh artis jepang lagu tersebut tersedia di [youtube](https://www.youtube.com/watch?v=5rHYh1MMIv0) di sini saya berasumsi bahwa flag nya ada di komentar youtube tersebut, lalu saya menggunakan [youtube comment finder](https://ytcomment.kmcat.uk/) dan mendapatkan jackpot berupa flag  

 ![flag oseen](/images/diskoctf/flag-oseen.png)  
**Flag: DiskoCTF{in_distant_memory}**
## Binary Exploitation
### Evil Input
>Tebak nomor berhadiah flag...
nc ctf.serangkota.go.id 9971
di berikan sebuah file python berisikan kodingan sebagai berikut :
```python
from __future__ import print_function
from random import randint

correct  = randint(1, 999999)

print("Tebak Nomor : ", end="")

question = input("")

if question == correct:
    import flag
else:
    print("Salah! Yang Bener", correct)
```
pada baris ke 9 ada bug input pada python 2.7 dan dapat di eksekusi langsung dengan sebagai berikut:  
```bash
[fallcrescent@ccug Downloads]$ nc ctf.serangkota.go.id 9971
Tebak Nomor : correct
DiskoCTF{bukan_tebak_tebakan}
```
**Flag: DiskoCTF{bukan_tebak_tebakan}**
