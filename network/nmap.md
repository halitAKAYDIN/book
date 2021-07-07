# Nmap

#### Firewall/IDS Evasion and Spoofing

```text

nmap -f 192.168.1.1 -p443 -PN -n
nmap --mtu 16 192.168.1.1 -p443 -PN -n --data-length 80
nmap -sS -T5 192.168.1.1 --script firewall-bypass    
```

#### Spoof source address

```text
nmap -sS -p80 192.168.1.1 -S 192.168.2.100 -e wlan0 -PN -n
```

#### Spoof source port number

```text
nmap -sS -p80 192.168.1.1 -g53 -PN -n
```

#### Spoof MAC address

```text
nmap -sP 192.168.1.1 — spoof-mac 00:11:22:33:44:55
```

#### Append random data to sent packets

```text
nmap -sS -p80 192.168.1.1 --data-length 100 -PN -n
```

#### This LAND Attack will crash vulnerable systems!

```text
nmap -sS 192.168.1.1 -S 192.168.1.1 -p139 -g139 -e wlan0
```

#### ICMP \(ping\) requests

```text
sudo nmap -PE 192.168.1.1
```

#### Perform an Xmas scan on the first 999 ports

```text
sudo nmap -sX -p 1-999 192.168.1.1 -Pn
```

#### Perform a TCP SYN scan on the first 5000 ports

```text
sudo nmap -sS -p 1-5000 --open -Pn 192.168.1.1
```

#### Login successfully to the FTP server on port 21

```text
nmap --script ftp-anon -p 192.168.1.1
```

#### SYN/ACK indicating an open port and RST indicating a closed port

```text
nmap 192.168.1.1 -p666 -sF --scanflags URGPSH
```

#### To start nmap’s os detection engine use a command as follows

```text
nmap -O 192.168.1.1 -p22 -n -PN 
```

| COMMAND | DESCRIPTION |
| :--- | :--- |
| `-sT` | TCP Connect\(\) Scan |
| `-sS` | SYN Scan |
| `-sS` | SYN Scan |
| `-sA` | ACK Scan |
| `-sW` | Window |
| `-sN` | Null Scan |
| `-sF` | FIN Scan |
| `-sX` | XMas Scan |
| `-sU` | UDP Scan |
| `-sM` | Maimon Scan |
| `-sO` | IP Protocol Scan |
| `-sI` | host:port Idle Scan |
| `-b` | FTP Bounce Scan |

## **References**

* \*\*\*\*[**https://nmap.org/**](https://nmap.org/)\*\*\*\*

