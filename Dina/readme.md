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
            
            #my secret pass
            freedom
            password
            helloworld!
            diana
            iloveroot
Busting the directories of the ip:

command:
        
            $dirb http://192.168.42.54 /usr/share/dirb/wordlists/big.txt

Result:

            -----------------
            DIRB v2.22    
            By The Dark Raver
            -----------------

            START_TIME: Sun May 28 20:45:04 2023
            URL_BASE: http://192.168.42.54/
            WORDLIST_FILES: /usr/share/dirb/wordlists/big.txt

            -----------------

            GENERATED WORDS: 20458                                                         

            ---- Scanning URL: http://192.168.42.54/ ----
            + http://192.168.42.54/cgi-bin/ (CODE:403|SIZE:289)                                                                
            + http://192.168.42.54/index (CODE:200|SIZE:3618)                                                                  
            ==> DIRECTORY: http://192.168.42.54/nothing/                                                                       
            + http://192.168.42.54/robots (CODE:200|SIZE:102)                                                                  
            + http://192.168.42.54/robots.txt (CODE:200|SIZE:102)                                                              
            ==> DIRECTORY: http://192.168.42.54/secure/                                                                        
            + http://192.168.42.54/server-status (CODE:403|SIZE:294)                                                           
            ==> DIRECTORY: http://192.168.42.54/tmp/                                                                           
            ==> DIRECTORY: http://192.168.42.54/uploads/                                                                       

            ---- Entering directory: http://192.168.42.54/nothing/ ----
            + http://192.168.42.54/nothing/index (CODE:200|SIZE:180)                                                           
            + http://192.168.42.54/nothing/pass (CODE:200|SIZE:57)                                                             

            ---- Entering directory: http://192.168.42.54/secure/ ----
            (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
                (Use mode '-w' if you want to scan it anyway)

            ---- Entering directory: http://192.168.42.54/tmp/ ----
            (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
                (Use mode '-w' if you want to scan it anyway)

            ---- Entering directory: http://192.168.42.54/uploads/ ----
            (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
                (Use mode '-w' if you want to scan it anyway)

            -----------------
            END_TIME: Sun May 28 20:45:09 2023
            DOWNLOADED: 40916 - FOUND: 7
We got a backup.zip inside the dir:

            http://192.168.42.54/secure/          
The zip is password locked and we have some wordlist from the nothing dir. We can try it with john or else manually typing it one by one.

The pass is:
