
2026-04-02 18:33

status: #Adult 

tags: [[3 - Tags/Arch|Arch]] [[Linux]] [[Hacking]] [[Spanish]] [[English]]

# Configuración de entorno

Para empezar debemos descargar la iso de arch, para ello debemos ir a https://archlinux.org/download/ y escoger el mirror mas cercano a nuestra region:

![[Pasted image 20260402183733.png]]
(en mi caso es Colombia)

Debemos descargar la .iso
![[Pasted image 20260402183849.png]]

Luego de instalarla inicializamos nuestro gestor de maquinas virtuales y lo configuramos. Lo ideal es usar los recursos adecuados para el proposito que vaya a tener el la maquina, asignar como minimo 4 de ram, cpu basicos, red en bridget adapter y demas.

Al iniciarlo debemos ejecutar el comando **archinstall**, este nos abrira una TUI basica para seleccionar paqueteria y opciones, estas son las que yo escogi:

![[Pasted image 20260402191811.png]]
![[Pasted image 20260402191827.png]]
![[Pasted image 20260402191844.png]]![[Pasted image 20260402191903.png]]
![[Pasted image 20260402191933.png]]
![[Pasted image 20260402191951.png]]
![[Pasted image 20260402192006.png]]
![[Pasted image 20260402192018.png]]

Luego de esto vamos a instalar la interfaz grafica que mas se utiliza de lightdm en linux

![[Pasted image 20260402200953.png]]

Y finalmente habilitamos este servicio
![[Pasted image 20260402201202.png]]


Los entornos de trabajo en arch linux se deben configurar desde 0, esto puede ser un retos si no se ha trabajado antes en linux o no se tiene experiencia avanzada. Por esto muchas personas crearon sus propias configuraciones (omarchy por ejemplo) para que solo tengamos que obtenerlas usuando una peticion curl y aplicando las configuraciones del repositorio:

![[Pasted image 20260402201732.png]]

Cuando inicie el proceso va a descargar toda la configuracion, debemos decirle que no al momento que nos pregunte si queremos usar su configuracion de nvim.

Finalmente despues de hacer reboot nos saldra lo siguiente:
![[Pasted image 20260402205449.png]]

Para quitarlo directamente debemos hacer lo siguiente:
```bash
nvim .zshrc
# Y comentamos la linea 116
# Tambien podemos comentar la linea de colorscript que esta al final del archivo

nvim .config/bspwm/bspwmrc
# aqui borramos las ultimas lineas para que no nos salga el mensaje del inicio
```

Si estamos en una maquina virtual debemos ejecutar los siguientes comandos para que se adapte al size de la pantalla:

```bash
sudo pacman -S open-vm-tools
sudo systemctl enable vmtoolsd.service
sudo systemctl enable vmware-vmblock-fuse.service
reboot
```




















### References


