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

# Pasar el teclado a es
loadkeys es
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
pacman -Syu
```

Instalar archinstall
```bash
archinstall
```

Archinstall nos va a mostrar un menu donde vamos realizar configuraciones de disco, preferencias y realizar la configuracion del escritorio que vamos a utilizar  --> hyprland.
Tambien podemos agregar aplicaciones y paquetes para que queden instalados en el sistema operativo.

Cuando se termine de realizar la instalación reiniciamos el pc, veremos que tenemos solo una consola como interfaz, aquí simplemente abriremos el navegador escribiendo **Firefox** y buscaremos el siguiente repo:


	https://github.com/HyDE-Project/HyDE

Seguimos los pasos de instalación y listo, ya tendríamos nuestro arch con hyprland bien cool :3