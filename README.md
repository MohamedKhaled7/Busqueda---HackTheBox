## Busqueda Writeup -- HackTheBox ##
  
** This is a walkthrough to get root access on a Linux machine called Busqueda from Hack The Box **

• Add the IP address on your hosts file 

**sudo gedit /etc/hosts**
![image](https://github.com/MohamedKhaled7/Busqueda---HackTheBox/assets/58820314/dee050f3-0dc5-41de-8e11-6f334472570f)

• Next step is to doing scanning for open ports and for service version using nmap and the command : **nmap -sV -A -T4 10.10.11.208**

