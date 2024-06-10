# Day 1 Schedule: Forensics

| Day | Time                  | Title                                                                 |   Instructor |
|-----|------------------------|-----------------------------------------------------------------------------|------------|
| 1    | 09:00-09:15 | Arrival |  |
| 1    | 09:15-09:30 | Welcome |  |
| 1    | 09:30-10:00  | Lesson: Cyber Laws | TJ       |
| 1    | 10:00-10:15  | Snack | |
| 1    | 10:15-10:45  | Introduction to Linux and the Command Line |   TJ        |
| 1    | 10:45-11:00 | Jeopardy Game |
| 1    | 11:00-11:30 | Challenges: Command Line on spacecadets.ctfd.io | |
| 1    | 11:30-12:30 | Lunch |
| 1    | 12:30-13:15  | Network Traffic Analysis  | TJ        |
| 1    | 13:15-13:45  | Challenges: Network Traffic Analysis on spacecadets.ctfd.io | |
| 1    | 13:45-14:30  |  Packet Crafting| TJ     |
| 1    | 14:30-15:00  | Challenges: Packet Crafting on spacecadets.ctfd.io | |
| 1    | 15:00-15:30  | Integrity Activity: Packet Wars |       |
| 1    | 15:30-16:00  | Journaling | |

# Resources
- [OverTheWire](https://overthewire.org/wargames/)
- [LinuxSurvival](https://linuxsurvival.com)
- [PwnTools](https://python3-pwntools.readthedocs.io/)
- [Netcat](https://netcat.sourceforge.net)
- [Tcpdump](https://www.tcpdump.org)

# Challenge Solutions

## Linux Command Line CTFd Exercise

Exercise (Linux Command Line):

**root-directory**: display the contents of the file named ``/root/gencyber/forensics/flag.txt`` 
```
cd /root/gencyber/forensics/
cat /root/gencyber/forensics/flag.txt 
flag{sp4c3_expl0r3r}

```

**linux-commands**: find a process thats running, whose name begins with ``flag{``
```
ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 293224  7036 pts/0    Ss+  14:02   0:00  /bin/tmux /bin/tmux
root         7  0.2  0.5 324964 40752 pts/0    S+   14:02   0:00  /usr/bin/python3 /usr/bin/python3 /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf
root         9  0.0  0.0 287344  4060 pts/0    S    14:02   0:00  /opt/giveme /opt/giveme
root        10  0.0  0.0 287200  3932 pts/0    S    14:02   0:00  /opt/flagping/flagping /opt/flagping/flagping
root        11  0.0  0.0 287192  3804 pts/0    S    14:02   0:00  /opt/flag{c0sm1c_h4ck} /opt/flag{c0sm1c_h4ck}
root        13  0.2  0.1 294152  9304 ?        Ss   14:02   0:00  /bin/tmux /bin/tmux
root        14  0.5  0.2 299936 16392 pts/1    Ss   14:02   0:00  /bin/zsh -zsh
root       196  100  0.0 295904  7516 pts/1    R+   14:05   0:00 /usr/bin/ps ps aux
```

**nano-editor**: edit the file named ``/root/gencyber/forensics/giveme.txt`` to contain the message ``theflag``. when done correctly, you will receive a new flag in ``/root/gencyber/forensics/theflag.txt``
```
nano giveme.txt 
cat theflag.txt 
flag{g4l4ctic_v0y4g3}#   
```

**find-flag**: there is a file named ``flag.txt`` hidden in a sub directory of the ``/usr/`` directory.  
```
find /usr -name "flag.txt" 
/usr/local/share/give/me/th3/fl4g/n0w/flag.txt

cat /usr/local/share/give/me/th3/fl4g/n0w/flag.txt
flag{s4t3ll1t3_intrusi0n}
```

**check-history**: check your history for the flag
```
 history
    1  echo flag{moonlighting_malware}
```

**flag-storage**: make a new directory called /root/gencyber/forensics/flagstorage; wait 10 seconds and then read the file named /root/gencyber/forensics/flagstorage/flag.txt
```
mkdir /root/gencyber/forensics/flagstorage         
cat /root/gencyber/forensics/flagstorage/flag.txt 
flag{astro_attack}#               
```

**flag-touch**: make a new file named /opt/touchme.txt
```
➭ touch /root/gencyber/forensics/touchme.txt
cat /root/gencyber/forensics/touchme.txt
flag{nebula_network}

```

## Networking Traffic Analysis CTFd Exercise

Networking challenges

**dump-traffic**: dump the network traffic on the network adapter ``lo`` to see who is sending ``127.0.0.1`` some ``icmp`` traffic containing a flag. 
```
tcpdump -i lo icmp -A
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lo, link-type EN10MB (Ethernet), snapshot length 262144 bytes
14:06:32.143174 IP localhost > localhost: ICMP echo request, id 4608, seq 0, length 52
E..H..@.@...................flag{4str0n4ut_1ntrud3r}....................
```

**tcp-port**: connect to ip address ``127.0.0.1``, on ``tcp port 1337``; the service will respond with the flag

```
nc 127.0.0.1 1337
flag{moon_landing}
```

**send-file**: send the contents of ``/root/gencyber/forensics/constitution.txt`` to ``tcp port 31337`` on ``127.0.0.1``; if done correctly, the file ``/root/gencyber/forensics/portflag.txt`` will appear and contain the flag 

```
cat constitution.txt | nc localhost 31337     
cat /root/gencyber/forensics/portflag.txt    
flag{rebel_alliance}#
```

**math-socket**: connect to ``tcp port 1984`` on ``127.0.0.1``' its hosting a series of math problems, that you will want to solve to get the flag; be quick! you have to do it in under 10 seconds

```
python3 solve-math.py
[+] Opening connection to 127.0.0.1 on port 1984: Done
Congratulations! Here's your flag: flag{fast_rocket}
```

- ping a server and it will respond with a flag

## Packet Crafting Exercise

**flag1:** Change to the flags directory using the command ``cd /flags`` and then type ``nano craft-pkt.py`` to create a new python3 script named ``craft-pkt.py``. Paste the following contents into the script, save it using ``ctrl-X`` and confirm with ``y``. Then run the script using the command ``python3 craft-pkt.py``. This script sends an *IP* packet to to *192.168.1.1* with the IP identifier set to *1984*.  If you did everything correctly, there will be a new file named ``/flags/flag1.txt``. Submit the flag

```python
from scapy.all import *

pkt = IP(dst='192.168.1.1',id=1984)
send(pkt)
```

**flag2:** Now, you must craft an IP packet to *192.168.1.2* with the *IP identifier set to 1984*  with a *TCP destination port of 31337*. If you do everything correctly, /flags/flag2.txt will appear. 

```python
from scapy.all import *

pkt = IP(dst='192.168.1.2',id=1984)/TCP(dport=31337)
send(pkt)
```

**flag3:** Now, you must craft an IP packet to *192.168.1.3*  with the *IP identifier set to 1984* with a *UDP destination port of 31337*. If you do everything correctly, /flags/flag3.txt will appear. 

```python
from scapy.all import *

pkt = IP(dst='192.168.1.3',id=1984)/UDP(dport=31337)
send(pkt)
```

**flag4:** Now, you must craft an IP packet to *192.168.1.4* with the *IP identifier set to 1984*  with a *TCP destination port for the TELNET service*. If you do everything correctly, /flags/flag4.txt will appear. 

```python
from scapy.all import *

pkt = IP(dst='192.168.1.4',id=1984)/TCP(dport=23)
send(pkt)
```

**flag5:** Now, you must craft an IP packet to *192.168.1.5* with the *IP identifier set to 1984*  with a *TCP destination port for the SSH service*. If you do everything correctly, /flags/flag5.txt will appear. 

```python
from scapy.all import *

pkt = IP(dst='192.168.1.5',id=1984)/TCP(dport=22)
send(pkt)
```

**flag6:** Now, you must craft an IP packet to *192.168.1.6* with the *IP identifier set to 1984*  with a *UDP destination port for the NTP service*. If you do everything correctly, /flags/flag6.txt will appear. 

```python
from scapy.all import *

pkt = IP(dst='192.168.1.6',id=1984)/UDP(dport=123)
send(pkt)
```

**flag7:** Now, you must craft an IP packet to *192.168.1.7* with the *IP identifier set to 1984*  and includes an ICMP echo request. If you do everything correctly, /flags/flag7.txt will appear. 

```python
from scapy.all import *

pkt = IP(dst='192.168.1.7',id=1984)/ICMP()
send(pkt)
````
