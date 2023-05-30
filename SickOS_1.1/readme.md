# SickOS V1.1

Netdiscover result:

     Currently scanning: 192.168.79.0/16   |   Screen View: Unique Hosts                                                 

     4 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 204                                                     
     _____________________________________________________________________________
       IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
     -----------------------------------------------------------------------------
     192.168.42.36   08:00:27:c0:67:ce      2     120  PCS Systemtechnik GmbH                                            
     192.168.42.129  42:fd:72:f9:05:21      2      84  Unknown vendor 


Nmap result:

    # nmap -sS -sV -p- -A 192.168.42.36
    Starting Nmap 7.93 ( https://nmap.org ) at 2023-05-30 16:38 IST
    Nmap scan report for 192.168.42.36
    Host is up (0.00057s latency).
    Not shown: 65532 filtered tcp ports (no-response)
    PORT     STATE  SERVICE    VERSION
    22/tcp   open   ssh        OpenSSH 5.9p1 Debian 5ubuntu1.1 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   1024 093d29a0da4814c165141e6a6c370409 (DSA)
    |   2048 8463e9a88e993348dbf6d581abf208ec (RSA)
    |_  256 51f6eb09f6b3e691ae36370cc8ee3427 (ECDSA)
    3128/tcp open   http-proxy Squid http proxy 3.1.19
    |_http-title: ERROR: The requested URL could not be retrieved
    | http-open-proxy: Potentially OPEN proxy.
    |_Methods supported: GET HEAD
    |_http-server-header: squid/3.1.19
    8080/tcp closed http-proxy
    MAC Address: 08:00:27:C0:67:CE (Oracle VirtualBox virtual NIC)
    Device type: general purpose
    Running: Linux 3.X|4.X
    OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
    OS details: Linux 3.2 - 4.9
    Network Distance: 1 hop
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    TRACEROUTE
    HOP RTT     ADDRESS
    1   0.57 ms 192.168.42.36

    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 133.50 seconds

Open the ip in the browser after changing the proxy of the browser to access the network.

![image](https://github.com/keerthiprabup/VM/assets/116485904/c2e942e1-fd49-4c21-b788-cdd0ab1daf1d)


We will get the ip page as:
![image](https://github.com/keerthiprabup/VM/assets/116485904/4adb1db3-1ec9-4ea4-ac86-9cb3e6f5c695)

Putting /robots.txt in the ip
![image](https://github.com/keerthiprabup/VM/assets/116485904/6bbf6789-8eee-4caf-b598-39cc92959a4b)

We got a dir /wolfcms

Open it by pasting it in the domain:

![image](https://github.com/keerthiprabup/VM/assets/116485904/7a217b67-a6c3-4467-bd7b-a19849c17677)

Wolf CMS is an open-source content management system (CMS) designed to make it easier for users to build and manage websites. 

Surfing through some wolfcms made websites we could figure out the pattern of url for login page:

    http://192.168.42.36/wolfcms/admin/login
But it won't work!

We can try passing it as an argument:

    http://192.168.42.36/wolfcms/?/admin/login
Now we got an login page:
![image](https://github.com/keerthiprabup/VM/assets/116485904/e2f59e78-f17b-4a4f-a7ad-7416fb9f4b45)

We can try some default sql passwords:

Here admin:admin worked!!

![image](https://github.com/keerthiprabup/VM/assets/116485904/b7d4b3c8-fa2d-49de-ad6c-43df0869be4d)


Now we are logged in.

Here we could upload files:
![image](https://github.com/keerthiprabup/VM/assets/116485904/74556301-1c31-4098-9667-3b5a6550b6a9)

The php reverse shell script:





