# JIS-CTF Walkthrough

Netdiscover result:

![image](https://github.com/keerthiprabup/VM/assets/116485904/9b9d52b8-df6a-41da-ae33-1e2dac3731b8)

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

