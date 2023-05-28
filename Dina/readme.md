# Dina Walkthrough

Use netdiscover to find the ip of the vm

Command:

      $netdiscover
      
Output:

    Currently scanning: 192.168.52.0/16   |   Screen View: Unique Hosts                                              

     2 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 84                                                   
     _____________________________________________________________________________
       IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
     -----------------------------------------------------------------------------
     192.168.42.54   08:00:27:84:8e:f5      1      42  PCS Systemtechnik GmbH                                         
     192.168.42.129  06:23:0e:e5:82:1a      1      42  Unknown vendor
     
We got the ip.addr==192.168.42.54

nmap results:

            $ sudo nmap -A -p- -sS -sV 192.168.42.54 
            Starting Nmap 7.93 ( https://nmap.org ) at 2023-05-28 20:33 IST
            Nmap scan report for 192.168.42.54
            Host is up (0.00054s latency).
            Not shown: 65534 closed tcp ports (reset)
            PORT   STATE SERVICE VERSION
            80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
            | http-robots.txt: 5 disallowed entries 
            |_/ange1 /angel1 /nothing /tmp /uploads
            |_http-title: Dina
            |_http-server-header: Apache/2.2.22 (Ubuntu)
            MAC Address: 08:00:27:84:8E:F5 (Oracle VirtualBox virtual NIC)
            Device type: general purpose
            Running: Linux 2.6.X|3.X
            OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
            OS details: Linux 2.6.32 - 3.5
            Network Distance: 1 hop

            TRACEROUTE
            HOP RTT     ADDRESS
            1   0.54 ms 192.168.42.54

            OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
            Nmap done: 1 IP address (1 host up) scanned in 8.52 seconds
            
Using the port 80 open the ip in browser,

Open the dir robots.txt

Result:

            User-agent: *
            Disallow: /ange1
            Disallow: /angel1
            Disallow: /nothing
            Disallow: /tmp
            Disallow: /uploads
            
            
We will get some password from the inspection on the link:

            http://192.168.42.54/nothing/
commented pass:
<!--
#my secret pass
freedom
password
helloworld!
diana
iloveroot
-->
      
