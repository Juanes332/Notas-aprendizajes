
2026-04-03 00:14

status: #Adult 

tags: [[3 - Tags/Arch|Arch]] [[Linux]] [[Hacking]] [[Spanish]] [[English]]


# Gestores de paquetes en arch

Los principales gestores de paquetes en arch son pacman, paru y yay.

```bash
# para instalar paru debemos clonar el repo:
git clone https://aur.archlinux.org/paru.git
makepkg -si

# para instalar yay debemos clonar el repo:
git clone https://aur.archlinux.org/yay.git
makepkg -si
```

Y finalmente podremos instalar nuestras herramientas:

```bash
paru -S aircrack-ng ffuf burpsuite nikto john hashcat hydra wireshark-qt tcpdump netcat metasploit exploitdb curl wget git jq python-pip hcxtools hcxdumptool bettercap sherlock recon-ng impacket proxychains-ng ghidra radare2 gdb pwndbg binwalk foremost volatility3 steghide exiftool openvpn
```



