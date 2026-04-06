
2026-04-02 21:04

status:  #Adult 

tags: [[3 - Tags/Arch|Arch]] [[Linux]] [[Hacking]] [[Spanish]] [[English]]


# Personalización visual y ajuste de configuraciones

Paths de configuraciones y comandos
**Comandos**
```bash
# para cambiar la terminal para usar kitty
super + alt + t

# para crear una terminal debajo de otra en kitty
ctrl + shift + enter

```

Para realizar personalizaciones del tema y entorno del trabajo

```bash
# configs de bspwn por temas
~/.config/bspwm/rices/themes

# dentro estan los archivos de configuracion
```

Archivo config
```bash
config.ini

# configuraciones de la polybar
```

Archivo modules
```bash
modules.ini

# configuraciones de modulos para importar a la polybar
```

Para editar los temas y configs de la terminal de kitty debemos usar editar/crear el archivo kitty.conf

```bash
enable_audio_bell no

include color.ini

font_family HackNerdFont
font_size 13

disable_ligatures never

url_color #61afef

url_style curly

map ctrl+left neighboring_window left
map ctrl+right neighboring_window right
map ctrl+up neighboring_window up
map ctrl+down neighboring_window down

map f1 copy_to_buffer a
map f2 paste_from_buffer a
map f3 copy_to_buffer b
map f4 paste_from_buffer b

cursor_shape beam
cursor_beam_thickness 1.8

mouse_hide_wait 3.0
detect_urls yes

repaint_delay 10
input_delay 3
sync_to_monitor yes

map ctrl+shift+z toggle_layout stack
tab_bar_style powerline

inactive_tab_background #e06c75
active_tab_background #98c379
inactive_tab_foreground #000000
tab_bar_margin_color black

map ctrl+shift+enter new_window_with_cwd
map ctrl+shift+t new_tab_with_cwd

background_opacity 0.95

shell zsh
```

Para los colores se usa color.ini
```bash
cursor_shape          Underline
cursor_underline_thickness 1
window_padding_width  20

# Special
foreground #a9b1d6
background #1a1b26

# Black
color0 #414868
color8 #414868

# Red
color1 #f7768e
color9 #f7768e

# Green
color2  #73daca
color10 #73daca

# Yellow
color3  #e0af68
color11 #e0af68

# Blue
color4  #7aa2f7
color12 #7aa2f7

# Magenta
color5  #bb9af7
color13 #bb9af7

# Cyan
color6  #7dcfff
color14 #7dcfff

# White
color7  #c0caf5
color15 #c0caf5

# Cursor
cursor #c0caf5
cursor_text_color #1a1b26

# Selection highlight
selection_foreground #7aa2f7
selection_background #28344a
```

Instalacion de fzf: https://github.com/junegunn/fzf

Se elimina la version anterior con el siguiente comando:

```bashrc 
sudo pacman -R fzf -y
```






