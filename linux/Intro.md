---
id: 2025112901
tags:
  - Linux
  - Cibersecurity
  - Operating_Sistem
  - Console
  - Commands
author: Juan Hernandez
type: permanent
---
## Que es un sistema operativo?

Un sistema operativo es un conjunto de programas que administran un sistema físico (hardware), protocolos de ejecución (software) y la interfaz de usuario.

En concreto para aprender hacking debemos usar sistemas operativos orientados a pentesting como estos dos:

- Enlace de descarga de Parrot: [https://parrotsec.org/download/](https://parrotsec.org/download/)
- Enlace de descarga de Kali Linux: [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)

En mi caso voy a usar Kali porque pues me gusta mas y ya esta.

Para instalarlo se recomienda bastante tener vmware o virtualbox (tambien QEMU o KVM).

---

## Comandos básicos de linux

**Chuleta de comandos:**
https://cibered.com/chuleta-comandos-linux/

---
## Control del flujo stderr-stdout, operadores y procesos en segundo plano

![[bash-redirections-cheat-sheet.pdf]]

```bash
# Ejecutar varios comandos per linea
whoami; ls; cat /usr/bin/whoami; sudo -i

# Conocer el codigo de estado del comando anterior
echo $?

# si sale 0 es porque todo salio bien, si no, salio mal
# Redireccionar stderr al /dev/null
whoamy 2> /dev/null # si lo hacemos de esta manera no nos saldra el error en consola

# Redireccionar stdout y stderr al devnull para no ver logs de ejecucion
wireshark &> /dev/null

# para poder tenerlo en segundo plano:
wireshark &> dev/null & disown

```

