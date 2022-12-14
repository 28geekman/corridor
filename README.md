# Corridor from tryhackme
If you want to try this out yourself click here: https://tryhackme.com/room/corridor
```
Corr-IDOR
```

This room was awesome for learning basics of "Insecure Direct Object References" aka "IDOR" created by JohnHammond {My virtual teacher on youtube}
https://www.youtube.com/channel/UCVeW9qkBjo3zosnqUbG7CFw

So let's jump right in,

## nmap scan
```
# Nmap 7.92 scan initiated Fri Sep 30 16:39:25 2022 as: nmap -sC -sT -sV -A -oA nmap/corridor 10.10.68.111
Nmap scan report for 10.10.68.111
Host is up (0.27s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
80/tcp open  http    Werkzeug httpd 2.0.3 (Python 3.10.2)
|_http-title: Corridor

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Sep 30 16:41:02 2022 -- 1 IP address (1 host up) scanned in 96.85 seconds

``` 
We see there is only one port open i.e.. 80 for http, running werkzeug which is python webserver framework I believe..

## Website
We see that there are some doors (IDORS I think??) that we must open

we can see we are going to some locations i,e.. some hashed directories
a snippet from source code

```
<area target="" alt="c4ca4238a0b923820dcc509a6f75849b" title="c4ca4238a0b923820dcc509a6f75849b" href="c4ca4238a0b923820dcc509a6f75849b" coords="257,893,258,332,325,351,325,860" shape="poly">
        <area target="" alt="c81e728d9d4c2f636f067f89cc14862c" title="c81e728d9d4c2f636f067f89cc14862c" href="c81e728d9d4c2f636f067f89cc14862c" coords="469,766,503,747,501,405,474,394" shape="poly">
        <area target="" alt="eccbc87e4b5ce2fe28308fd9f2a7baf3" title="eccbc87e4b5ce2fe28308fd9f2a7baf3" href="eccbc87e4b5ce2fe28308fd9f2a7baf3" coords="585,698,598,691,593,429,584,421" shape="poly">
        <area target="" alt="a87ff679a2f3e71d9181a67b7542122c" title="a87ff679a2f3e71d9181a67b7542122c" href="a87ff679a2f3e71d9181a67b7542122c" coords="650,658,644,437,658,652,655,437" shape="poly">
        <area target="" alt="e4da3b7fbbce2345d7772b0674a318d5" title="e4da3b7fbbce2345d7772b0674a318d5" href="e4da3b7fbbce2345d7772b0674a318d5" coords="692,637,690,455,695,628,695,467" shape="poly">
        <area target="" alt="1679091c5a880faf6fb5e6087eb1b2dc" title="1679091c5a880faf6fb5e6087eb1b2dc" href="1679091c5a880faf6fb5e6087eb1b2dc" coords="719,620,719,458,728,471,728,609" shape="poly">
        <area target="" alt="8f14e45fceea167a5a36dedd4bea2543" title="8f14e45fceea167a5a36dedd4bea2543" href="8f14e45fceea167a5a36dedd4bea2543" coords="857,612,933,610,936,456,852,455" shape="poly">
        <area target="" alt="c9f0f895fb98ab9159f51fd0297e236d" title="c9f0f895fb98ab9159f51fd0297e236d" href="c9f0f895fb98ab9159f51fd0297e236d" coords="1475,857,1473,354,1537,335,1541,901" shape="poly">
        <area target="" alt="45c48cce2e2d7fbdea1afc51c7c6ad26" title="45c48cce2e2d7fbdea1afc51c7c6ad26" href="45c48cce2e2d7fbdea1afc51c7c6ad26" coords="1324,766,1300,752,1303,401,1325,397" shape="poly">
        <area target="" alt="d3d9446802a44259755d38e6d163e820" title="d3d9446802a44259755d38e6d163e820" href="d3d9446802a44259755d38e6d163e820" coords="1202,695,1217,704,1222,423,1203,423" shape="poly">
        <area target="" alt="6512bd43d9caa6e02c990b0a82652dca" title="6512bd43d9caa6e02c990b0a82652dca" href="6512bd43d9caa6e02c990b0a82652dca" coords="1154,668,1146,661,1144,442,1157,442" shape="poly">
        <area target="" alt="c20ad4d76fe97759aa27a0c99bff6710" title="c20ad4d76fe97759aa27a0c99bff6710" href="c20ad4d76fe97759aa27a0c99bff6710" coords="1105,628,1116,633,1113,447,1102,447" shape="poly">
        <area target="" alt="c51ce410c124a10e0db5e4b97fc2af39" title="c51ce410c124a10e0db5e4b97fc2af39" href="c51ce410c124a10e0db5e4b97fc2af39" coords="1073,609,1081,620,1082,459,1073,463" shape="poly">
```
let's look at those directories
c4ca4238a0b923820dcc509a6f75849b
...
..
c51ce410c124a10e0db5e4b97fc2af39

## md5
these directories looks like a md5 hash
so let's try to crack it with crackstation: https://crackstation.net/
those directories are ordered in sequence like 1,2,3,4,...,13
so c4ca4238a0b923820dcc509a6f75849b == 1 in md5

## exploiting IDOR
with this clue we can create a wordlist of potential md5 hashes (integers) let's try it out

I created a wordlist of md5 hashed numbers from 0 to 30
you can download it above "wordlist.txt" 

## Dir fuzzing with gobuster
```
/a87ff679a2f3e71d9181a67b7542122c (Status: 200) [Size: 632]
/8f14e45fceea167a5a36dedd4bea2543 (Status: 200) [Size: 632]
/c9f0f895fb98ab9159f51fd0297e236d (Status: 200) [Size: 632]
/1679091c5a880faf6fb5e6087eb1b2dc (Status: 200) [Size: 632]
/eccbc87e4b5ce2fe28308fd9f2a7baf3 (Status: 200) [Size: 632]
/c81e728d9d4c2f636f067f89cc14862c (Status: 200) [Size: 632]
/45c48cce2e2d7fbdea1afc51c7c6ad26 (Status: 200) [Size: 632]
/a87ff679a2f3e71d9181a67b7542122c (Status: 200) [Size: 632]
/c4ca4238a0b923820dcc509a6f75849b (Status: 200) [Size: 632]
/cfcd20[REDACTED]]9f98764da (Status: 200) [Size: 797]
/c20ad4d76fe97759aa27a0c99bff6710 (Status: 200) [Size: 632]
/d3d9446802a44259755d38e6d163e820 (Status: 200) [Size: 632]
/6512bd43d9caa6e02c990b0a82652dca (Status: 200) [Size: 632]
```


we got some hit, some may be of some default page

But, If we see the size  "cfcd2084[REDACTED]]7dff9f98764da" is bit odd (Size: 797)

this the md5 hash of number == 0 {Which really made me mad, I was trying from 1}

let's go there and capture the flag
```
flag{REDACTED}  {Don't hope any flags from me}
```
