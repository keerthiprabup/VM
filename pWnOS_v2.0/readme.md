# pWnOS_v2.0

Set your ip in the range 10.10.10.0/24

We got the ip of the VM from the challenge description

nmap results:

    # nmap -A -p- -sS 10.10.10.100 -sV
    Starting Nmap 7.93 ( https://nmap.org ) at 2023-05-28 18:25 IST
    Nmap scan report for 10.10.10.100
    Host is up (0.00046s latency).
    Not shown: 65533 closed tcp ports (reset)
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 5.8p1 Debian 1ubuntu3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   1024 85d32b0109427b204e30036dd18f95ff (DSA)
    |   2048 307a319a1bb817e715df89920ecd5828 (RSA)
    |_  256 1012644b7dff6a87372638b1449fcf5e (ECDSA)
    80/tcp open  http    Apache httpd 2.2.17 ((Ubuntu))
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    |_http-title: Welcome to this Site!
    |_http-server-header: Apache/2.2.17 (Ubuntu)
    MAC Address: 08:00:27:B9:17:B6 (Oracle VirtualBox virtual NIC)
    Device type: general purpose
    Running: Linux 2.6.X|3.X
    OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
    OS details: Linux 2.6.32 - 2.6.39, Linux 2.6.38 - 3.0
    Network Distance: 1 hop
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    TRACEROUTE
    HOP RTT     ADDRESS
    1   0.46 ms 10.10.10.100

    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 8.52 seconds

Busting directories on the domain 10.10.10.100 using the wordlist big.txt

command:
    
       $dirb -host http://10.10.10.100/ /usr/share/dirb/wordlists/big.txt
       
       
result:

    -----------------
    DIRB v2.22    
    By The Dark Raver
    -----------------

    START_TIME: Sun May 28 18:31:58 2023
    URL_BASE: http://10.10.10.100/
    WORDLIST_FILES: /usr/share/dirb/wordlists/big.txt

    -----------------

    GENERATED WORDS: 20458                                                         

    ---- Scanning URL: http://10.10.10.100/ ----
    + http://10.10.10.100/activate (CODE:302|SIZE:0)                                                                   
    ==> DIRECTORY: http://10.10.10.100/blog/                                                                           
    + http://10.10.10.100/cgi-bin/ (CODE:403|SIZE:288)                                                                 
    ==> DIRECTORY: http://10.10.10.100/includes/                                                                       
    + http://10.10.10.100/index (CODE:200|SIZE:854)                                                                    
    + http://10.10.10.100/info (CODE:200|SIZE:50173)                                                                   
    + http://10.10.10.100/login (CODE:200|SIZE:1174)                                                                   
    + http://10.10.10.100/register (CODE:200|SIZE:1562)                                                                
    + http://10.10.10.100/server-status (CODE:403|SIZE:293)                                                            

    ---- Entering directory: http://10.10.10.100/blog/ ----
    + http://10.10.10.100/blog/add (CODE:302|SIZE:0)                                                                   
    + http://10.10.10.100/blog/atom (CODE:200|SIZE:1062)                                                               
    + http://10.10.10.100/blog/categories (CODE:302|SIZE:0)                                                            
    + http://10.10.10.100/blog/colors (CODE:302|SIZE:0)                                                                
    + http://10.10.10.100/blog/comments (CODE:302|SIZE:0)                                                              
    ==> DIRECTORY: http://10.10.10.100/blog/config/                                                                    
    + http://10.10.10.100/blog/contact (CODE:200|SIZE:5908)                                                            
    ==> DIRECTORY: http://10.10.10.100/blog/content/                                                                   
    + http://10.10.10.100/blog/delete (CODE:302|SIZE:0)                                                                
    ==> DIRECTORY: http://10.10.10.100/blog/docs/                                                                      
    ==> DIRECTORY: http://10.10.10.100/blog/flash/                                                                     
    ==> DIRECTORY: http://10.10.10.100/blog/images/                                                                    
    + http://10.10.10.100/blog/index (CODE:200|SIZE:8094)                                                              
    + http://10.10.10.100/blog/info (CODE:302|SIZE:0)                                                                  
    ==> DIRECTORY: http://10.10.10.100/blog/interface/                                                                 
    ==> DIRECTORY: http://10.10.10.100/blog/languages/                                                                 
    + http://10.10.10.100/blog/login (CODE:200|SIZE:5657)                                                              
    + http://10.10.10.100/blog/logout (CODE:302|SIZE:0)                                                                
    + http://10.10.10.100/blog/options (CODE:302|SIZE:0)                                                               
    + http://10.10.10.100/blog/rdf (CODE:200|SIZE:1411)                                                                
    + http://10.10.10.100/blog/rss (CODE:200|SIZE:1237)                                                                
    ==> DIRECTORY: http://10.10.10.100/blog/scripts/                                                                   
    + http://10.10.10.100/blog/search (CODE:200|SIZE:4940)                                                             
    + http://10.10.10.100/blog/setup (CODE:302|SIZE:0)                                                                 
    + http://10.10.10.100/blog/static (CODE:302|SIZE:0)                                                                
    + http://10.10.10.100/blog/stats (CODE:200|SIZE:5299)                                                              
    ==> DIRECTORY: http://10.10.10.100/blog/themes/                                                                    
    + http://10.10.10.100/blog/trackback (CODE:302|SIZE:0)                                                             
    + http://10.10.10.100/blog/upgrade (CODE:302|SIZE:0)                                                               
    + http://10.10.10.100/blog/upload_img (CODE:302|SIZE:0)                                                            

    ---- Entering directory: http://10.10.10.100/includes/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/config/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/content/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/docs/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/flash/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/images/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/interface/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/languages/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/scripts/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    ---- Entering directory: http://10.10.10.100/blog/themes/ ----
    (!) WARNING: Directory IS LISTABLE. No need to scan it.                        
        (Use mode '-w' if you want to scan it anyway)

    -----------------
    END_TIME: Sun May 28 18:32:07 2023
    DOWNLOADED: 40916 - FOUND: 28
We link from it having images in it:

    http://10.10.10.100/blog/images/
    
Browsing the link redirects to login to continue.

The source code shows us that the website is running on 
