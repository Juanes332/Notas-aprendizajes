---
id: 20250523
type: permanent
tags:
  - Linux
author: juan
---
## Configuracion inicial

Configurar el size de la ventana
```bash
setfont ter-118n
```

Conectarse a Internet
```bash
iwctl
device list
device wlan0 show
station wlan0 get-networks
station wlan0 connect NameWifiRed
```

Crear la key para repos
```bash
pacman-key --init
pacman --populate archlinux
```

Instalar archinstall
```bash
archinstall
```

Dentro de hyprland escribir
```bash
sudo nmcli dev wifi connect WifiName password "WifiPassword"
```


