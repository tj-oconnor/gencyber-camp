# Day 4 Schedule: Reverse Engineering (RE)

Schedule Change Pending

| Day | Time                  | Title                                                                 |   Instructor |
|-----|------------------------|-----------------------------------------------------------------------|-------------|
| 4   | 09:00-09:15 | Arrival |
| 4   | 09:15-09:55                       | Intro to Machine Code | TJ      |
| 4   | 09:55-10:25 | Intro Chals on spaccadets.ctfd.io | |
| 4   | 10:25-10:40 | Snack | |
| 4    | 10:40-11:20                       | Dynamic Analysis | TJ     |
| 4    | 11:20-11:50 | Dynamic Chals on spacecadets.ctfd.io | |
| 4    | 11:50-12:50 | Lunch | |
| 4    | 12:50-13:30                       | | TJ      |
| 4    | 13:10-13:40                       | Constraint Solving & Symbolic Execution | TJ        |
| 4    | 13:40-14:10 | Symbolic Execution Chals on spacecadets.ctfd.io | |
| 4    | 14:10-15:40 | Activity: RansomWare Challenge                                  |        |
| 4    | 15:40-16:00 | Journaling | |

# Resources

- [https://dogbolt.org/](https://dogbolt.org/)
- [https://godbolt.org/](https://godbolt.org/)
- [Binary Ninja Cloud](https://cloud.binary.ninja)
- [Live Overflow](https://www.youtube.com/@LiveOverflow)
- [GDB](https://web.eecs.umich.edu/~sugih/pointers/gdbQS.html)
- [PwnDbg](https://github.com/pwndbg/pwndbg)
- [NSA CodeBreaker](https://nsa-codebreaker.org/resources)
- [movfuscator](https://github.com/xoreaxeaxeax/movfuscator)
- [angr](https://angr.io)
- [Angr CheatSheet](https://github.com/angr/angr-doc/blob/master/CHEATSHEET.md)



# Challenge Solutions
***apollo-1**

```
python3 chal.py
````

once it starts up, type ``c`` to continue and examine the state of the ``rdi`` and ``rsi`` registers


**apollo-2**

stat the challenge with the debugger
```
gdb ./chal.bin
```

break on the specific address
```
gdb >> break *0x4013e7
gdb >> run
```

and then examine the state of the registers

**apollo-3** ``ltrace ./chal.bin`` and look for the strcmp

** Ransomware**: If you look [here](https://dogbolt.org/?id=5b9c0700-0e29-4f80-acfa-5d8528ff3766#), you should see that the dropper.bin downloads encrypt.bin from a webdav server. Making a logically leap that if the encrypt.bin tool is encrypting, the ``decrypt.bin`` must decrypt, you can issue the curl command to download the decryption tool with the correct username and password ``curl -u armstrong:the3agl3HasLand3d -O https://spacecadets-blog.chals.io/webdav/decrypt.bin``. Running the decryption tool, it has a strcmp for the password, which is ``0n3G1antL3ap``. As long as ``launch-codes.enc`` is in the current directory, download decrypt.bin, ``chmod +x decrypt.bin; ./decrypt.bin`` should produce a new file called ``launch-codes.txt`` that contains the flag. 

Full solution

```
$ curl -u guest:password1234 -O https://spacecadets-helpdesk.chals.io/webdav/dropper.bin
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 18312  100 18312    0     0  56421      0 --:--:-- --:--:-- --:--:-- 57046

$ curl -u guest:password1234 -O https://spacecadets-helpdesk.chals.io/webdav/launch-codes.enc
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    70  100    70    0     0    309      0 --:--:-- --:--:-- --:--:--   315

$ curl -u armstrong:the3agl3HasLand3d -O https://spacecadets-blog.chals.io/webdav/decrypt.bin
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 14552  100 14552    0     0  37650      0 --:--:-- --:--:-- --:--:-- 38094

$ chmod +x decrypt.bin
{13:04}~/gencyber/ctf/evidence ➭ ./decrypt.bin 
<<< Provide your password: 0n3G1antL3ap
launch-codes.txt decrypted successfully.

$ cat launch-codes.txt 
..........................flag{321_blast_off}...................6u#          
```
