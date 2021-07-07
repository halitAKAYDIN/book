---
description: 'Supports JPEG, BMP, WAV and AU file formats'
---

# Steghide

Displays info about a file whether it has embedded data or not.

```bash
steghide --info example.jpg
```

Extracts embedded data from a file

```bash
steghide extract -sf example.jpg
```

Extracts embedded data from a file using password

```bash
steghide embed -ef rockyou.txt -cf example.jpg
```

For more

```bash
man steghide
```

