nmap things

                └─$ sudo nmap -sV -sS -p- -O 192.168.42.92
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







http 9090 flag:
1.FLAG {THERE IS NO ZEUS, IN YOUR FACE!}



ftp flag:
2.FLAG{Whoa this is unexpected}






command sudo nmap -sV -sS -p- -O 192.168.42.92
Flag from syn fingerprint scan:
3.FLAG:{TheyFoundMyBackDoorMorty}



password from mortys: winter

flag from morty pass
4.FLAG{Yeah d- just don't do it.} 


uname: Summer
pass: winter
5.FLAG{Get off the high road Summer!} 



reverse shell flag:
6.FLAG{Flip the pickle Morty!} - 10 Points


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
