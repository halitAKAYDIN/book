# John the Ripper

## Hash Examples

### SHA1

```bash
john --wordlist=rockyou.txt --format=raw-sha1 crack.txt
```

### MD5

```bash
john --wordlist=rockyou.txt --format=raw-md5 rack.txt
```

### MD4

```bash
john --wordlist=rockyou.txt --format=raw-md4 rack.txt
```

### SHA256

```bash
john --wordlist=rockyou.txt --format=raw-sha256 rack.txt
```

### View All Formats

```bash
john --list=formats
```

## Stopping and Restoring Cracking

### Stop

While John the Ripper is booting, you can stop cracking by pressing "Ctrl+C" or "q".

### Restore

```bash
john --restore
```

