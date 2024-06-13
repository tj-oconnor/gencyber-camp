# Day 3 Schedule: Cryptography

| Day | Time                  | Title                                                                 |   Instructor |
|-----|------------------------|-----------------------------------------------------------------------------|------------|
| 3   | 09:00 - 09:15 | Reception / Welcome | | |
| 3   | 09:15 - 09:45 |  Lesson: Encoding Schemes |    Jared        |
| 3   | 09:45 - 10:00 | Practice:  Encoding on Spacecadets.ctfd.io |  |
| 3   | 10:00 - 10:15 | Snack |  |
| 3   | 10:15 - 10:45 | Lesson: Cryptography Intro    | Phil        |
| 3   | 10:45 - 11:15 | Practice: Cryptography on Spacecadets.ctfd.io |  |
| 3   | 11:15 - 12:00 | Lesson: Cryptographic Attacks|    TJ        |
| 3   | 12:00 - 12:30 | Practice: Attacks on Spacecadets.ctfd.io |  |
| 3   | 12:30 - 13:30  | Lunch |  |
| 3   | 13:30 - 14:00   | Activity: Crypto with Cars | | |
| 3   | 14:00 - 14:45   | Lesson: Password Cracking |    TJ        |
| 3   | 14:45 - 15:15   | Practice: Password Cracking on Spacecadets.ctfd.io|  |
| 3   | 15:15 - 15:30   | Demo: Drones Password Cracking |  |
| 3    | 15:30 - 16:00                       | Journaling          |            |

# Resources

- [Binary to Decimal E-Mates](https://d2hie3dpn9wvbb.cloudfront.net/binaryIP/binaryIP.html)
- [Hex Numbers E-Mates](https://d2hie3dpn9wvbb.cloudfront.net/hexadecimal/hexadecimal.html)
- [CyberChef](https://cyberchef.io)
- [Crunch](https://www.kali.org/tools/crunch/)
- [Cupp](https://github.com/Mebus/cupp)
- [John The Ripper](https://www.openwall.com/john/)
- [CryptoTool](https://www.cryptool.org/en/)
- [XorTool](https://github.com/hellman/xortool)
- [Dcode Monoalphabetic Substitution Solver](https://www.dcode.fr/monoalphabetic-substitution)
- [Cryptoggraph E-Mates](https://d2hie3dpn9wvbb.cloudfront.net/cryptography/cryptography.html)
- [Diffie Hellman E-Mates](https://d2hie3dpn9wvbb.cloudfront.net/diffie_hellman/diffie_hellman.html)
- [Hashing E-Mates](https://d2hie3dpn9wvbb.cloudfront.net/hashing/Hashing.html)

# Solutions

## Encoding Schemes

**binary**: decode the following flag from binary to ASCII

``01100110 01101100 01100001 01100111 01111011 01100100 01100101 01100001 01110100 01101000 01110011 01110100 01100001 01110010 01011111 01101000 01100001 01100011 01101011 01111101``

flag{deathstar_hack}

**decimal**: decode the following message from decimal to ASCII

``102 108 97 103 123 103 97 108 97 99 116 105 99 95 101 109 112 105 114 101 125``

flag{galactic_empire}

**hex**: decode the following message from hex to ASCII

``66 6c 61 67 7b 73 70 6f 63 6b 5f 6c 6f 67 69 63 7d`` 

flag{spock_logic}

## Cryptography

**Alices Secret Key**: This part of the challenge gives you ``B,a,p`` or the information Alice would typically have in his possesion (Bobs's Public Key, Alices Private Key, and the prime). Using the equation B^a mod p, we can calculate K. The python3 function pow(B,a,p) will compute B^a mod p. So we can open a python3 interpreter and solve for K.

```
>>> B=11693485118907663752341507099288171922374319336459760297769975369692679505931164608495283863386047801552888213246693230375801488174157618183009539527755001
>>> a=4010152298897977107110807047304318728774769073561856820094075276831196007776806715533803755163358525045827684966353515653261315972283943912016537532448242
>>> p=13021403767030239480657890800726885899411559353896124791013993357931946575988561113924189675811531018693428968652173983873545968711677307836581307926214279
>>> K=pow(B,a,p)
>>> K
8856786119312145470287314016547811200189930356193594521465854891244951202169502175053006374499811034263785175388316818970954074243510838810590785692727780
```

**Bobs Secret Key**: This part of the challenge gives you ``A,b,p`` or the information Bob would typically have in his possesion (Alice's Public Key, Bobs Private Key, and the prime). Using the equation A^b mod p, we can calculate K. The python3 function pow(A,b,p) will compute A^b mod p. So we can open a python3 interpreter and solve for K.

```
>>> A=11693485118907663752341507099288171922374319336459760297769975369692679505931164608495283863386047801552888213246693230375801488174157618183009539527755001
>>> b=4010152298897977107110807047304318728774769073561856820094075276831196007776806715533803755163358525045827684966353515653261315972283943912016537532448242
>>> p=13021403767030239480657890800726885899411559353896124791013993357931946575988561113924189675811531018693428968652173983873545968711677307836581307926214279
>>> K=pow(A,b,p)
>>> K
8856786119312145470287314016547811200189930356193594521465854891244951202169502175053006374499811034263785175388316818970954074243510838810590785692727780
```

**Two many secrets**: This time, you are given Bob and Alices secret keys and asked to solve for K. K can either equal ``K = pow(pow(g,b,p),a,p)`` or ``K = pow(pow(g,a,p),b,p)`` since we have both private keys. 

```
>>> g = 2
>>> a = 5473602363610171658132435982279839637383319232966200118542122108483265082749345711952166475322650607448832815042415681151327904781119706160951487391154492
>>> p = 12860067085142620734318950245053641683905324165749880769541885450849477098837703902802417214892968081603710546723823072136643342232903494329975719025716767
>>> b = 4996803973704180327922242647234681791093754785891376218931550970852916320237741086376002925244728459150764216860335316810617773028225439932589106011316371
>>> B = pow(g,b,p)
>>> K = pow(B,a,p)
>>> K
2495106147524019849374662056511213129738786449434128211886016507296249913265876256307111444109634797907536638466301745966098727643358512515130882744836066
```

## Attacks

**xor(xor(%PDF, flag),%PDF)**: Change into the ``/root/gencyber/crypto/pdf`` directory. The file ``flag.enc`` is originally a PDF document that has been encrypted by XORign the document with a 4-byte key. Recover the flag. 

We can use CyberChef to upload the file and then XOR the first four bytes with the filetype ``%PDF``. This should produce the key since XOR(cyphertext,plaintext) = key. We see the key is NASA and can then decrypt the entire document using this approach. 

**xor(0x0,key)**: Change into the ``/root/gencyber/crypto/bin`` directory. ``file.enc`` is a file that has been encrypted by taking a compiled linux binary and XORing it with a random 5-byte key. Discover the key and decrypt the file and run it. 

We can see that the file has the string ``SPACE`` repeating throughout the file. We can assumign this is the key that has been XORd(0x000000000,SPACE)=SPACE
```
{16:17}~/gencyber/crypto/bin ➭ hexdump -C flag.enc | head -5             
00000000  2c 15 0d 05 47 52 51 41  43 45 53 50 41 43 45 53  |,...GRQACESPACES|
00000010  52 41 7d 45 52 50 41 43  25 43 10 41 43 45 53 50  |RA}ERPAC%C.ACESP|
00000020  01 43 45 53 50 41 43 45  8b 66 41 43 45 53 50 41  |.CESPACE.fACESPA|
00000030  43 45 53 50 01 43 7d 53  5d 41 03 45 4d 50 5c 43  |CESP.C}S]A.EMP\C|
00000040  43 53 50 41 47 45 53 50  01 43 45 53 50 41 43 45  |CSPAGESP.CESPACE|
```

We can then decrypt the file using the following commands, make it executable, and display the flag.
```
{16:16}~/gencyber/crypto/bin ➭ xortool-xor -s SPACE -f ./flag.enc > flag.bin
{16:16}~/gencyber/crypto/bin ➭ chmod +x ./flag.bin        
{16:17}~/gencyber/crypto/bin ➭ ./flag.bin                                   
flag{apollo_mission_11}#                      
```

**Up a stream without a key**: use your gencyber docker machine to change into the ``/root/gencyber/forensics/atlantis-0`` directory and run the command ``python3 atlantis-0.py`` to connect to a server streaming an encrypted message.

Heres the problem. We dont tell you the key. Instead whats being streamed is a series of encrypted flag bytes. Each flag transmission is separated by a random anount of null bytes (0x00), which are also encrypted. Good luck!

Solving this relies on knowing the XOR(x,0x0) = x for all cases of x. So the key is the most prevelant byte in the stream. After that, you can just use cyberchef to solve it. 

**Up a stream without a !key**: use your gencyber docker machine to change into the ``/root/gencyber/forensics/atlantis-4`` directory and run the command ``python3 atlantis-4.py`` to connect to a server streaming an encrypted message.

Heres the problem. We dont tell you the key. Instead whats being streamed is a series of encrypted flag bytes. Each flag transmission is separated by a random anount of null bytes (0xff), which are also encrypted. Good luck!

Solving this relies on knowing the XOR(x,0xff) = !x for all cases of x. So the xor(key is the most prevelant byte in the stream,0xff). After that, you can just use cyberchef to solve it. 

**Up a stream with a random key**: To solve this one, rely on the fact that you know the flag format is flag{

Take the ciphertext, such as
```
62 68 65 63 7f 63 65 68 65 67 70 6d 67 5b 72 6b 7d 65 63 61 76 5b 3d 3d 79 0e 31 b5 dd d0 9c 60 65 18 e4 49 62 68 65 63 7f 63 65 68 65 67 70 6d 67 5b 72 6b 7d 65 63 61 76 5b 3d 3d 79 0e ae 83 11 aa df 10 5a a9 26 bb 30 6f 4c eb 25 11 d7 f8 c4 62 68 65 63 7f 63 65 68 65 67 70 6d 67 5b 72 6b 7d 65 63 61 76 5b 3d 3d 79 0e 5c 81 b2 2d 6f 42 ce 97 f8 0c 72 c5 4a 82 14 85 7c 62 68 65 63 7f 63 65 68 65 67 70 6d 67 5b 72 6b 7d 65 63 61 76 5b 3d 3d 79 0e c0 c7 6e 1b bc c8 9c e1 46 fb bb 34 a9 0b 07 35 9c 4d 54 62 68 65 63 7f 63 65 68 65 67 70 6d 67 5b 72
```
XOR it with flag{ using cyberchef and look for a repeating single character that repeats 5 times. Thats the key. In the case above, its 0x04. 

```
XOR(0x4, ciphertext) = plaintext
```

## Password Cracking

**Dwayne Smith**: Use ``cd /root/gencyber/crypto/cupp; python3 cupp.py -i`` to build a dictionary for dwayne smith knowing that his wife is named Jennie (JJ), their daughter is Sarah, and Jennis Birthday is 05221984 and Sarash is 05222019. Then run the command ``john hash.txt -w=dwayne.txt`` where hash.txt contains the hash for rocky from the ``/etc/shadow`` file and ``dwayne.txt`` is the password list created by cupp.

**Sally**: Logging onto the server we get sallys md5 hash by selecting ``3) History``. Also testing 2), we see that the password is only 6 characters long. We can crack this by creating a custom wordlist in crunch using ``crunch 6 6 > words.txt``. Then we can past the md5 hash into a file named hash2.txt and crack it using ``john --format=raw-md5 hash2.txt -w=words.txt``.

**Speedy Racer**: Similar to dwayne, we can create a custom wordlist with cupp and crach it with ``john hash3.txt -w=speedy.txt`` where speedy.txt is the wordlist created from cupp.py.
