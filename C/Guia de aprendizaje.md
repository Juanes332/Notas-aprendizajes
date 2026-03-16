
2026-02-21 13:36

status: #Adult

tags: [[Coding]] [[Algorithms]] [[Low Level]] [[Linux]]


# El modelo mental primero (lo que realmente vas a aprender)

Cuando programas en C estás aprendiendo simultáneamente:

- memoria (stack vs heap)
- layout real de datos
- syscalls del SO
- networking sin frameworks
- parsing manual
- concurrencia real
- performance sin magia

Eso te convierte naturalmente en backend engineer fuerte, porque entiendes lo que otros lenguajes esconden.

---

# ⚙️ Fase 0 — Setup del guerrero (Semana 1)

Antes de escribir proyectos.

Aprende solo lo mínimo necesario:

### Conceptos obligatorios

- compilación (`gcc`, `clang`)
- proceso compile → link → binary
- headers `.h`
- Makefile básico
- tipos primitivos
- punteros (sin obsesionarse aún)

### Herramientas (no negociables)

Instala y usa desde el día 1:

```
gcc / clang
gdb
valgrind
make
```

Tu mantra ya lo dijiste:

> use valgrind religiously

Porque en C los bugs **no fallan inmediatamente**. Se incuban.

---

# 🧱 Fase 1 — Sobrevivir a C (Semanas 2–4)

Aquí construyes reflejos básicos.

## Proyecto 1 — Notes Manager CLI (sin SQLite todavía)

Primero versión simple:

```
notes add "texto"
notes list
notes delete
```

Aprendes:

- strings manuales
- arrays dinámicos
- structs
- file I/O (`fopen`, `fread`, `fwrite`)
- malloc/free

Conceptos clave:

```
char*
struct
malloc()
free()
sizeof()
```

Aquí ocurre la primera iluminación:

👉 una string en C = puntero a memoria.

---

## Estructuras de datos (en paralelo)

Implementa tú mismo:

- dynamic array (vector)    
- linked list
- stack
- queue
- hash table básica

No copies código. Escríbelo y rómpelo.

Esto entrena:

- manejo de memoria
- ownership
- diseño de APIs en C


---
# 🔬 Fase 2 — Entender cómo leen los lenguajes (Semanas 5–7)

Aquí empiezas a pensar como compilador.

## Proyecto 2 — Lexical Analyzer

Lee texto y genera tokens.

Aprendes:

- parsing manual
- buffers
- finite state machines
- manejo eficiente de strings

Esto desbloquea TODO backend serio después.

---

## Proyecto 3 — Arithmetic Compiler (mini)

Ejemplo:

```
2 + 3 * 4
```

→ AST  
→ evaluación

Conceptos:

- árboles (AST)
- recursión real
- parsers
- separación lexer/parser

Ahora empiezas a pensar en estructuras complejas en memoria.

---

# 🌐 Fase 3 — Networking real (Semanas 8–12)

Aquí nace el backend engineer.

## Proyecto 4 — UDP Client/Server

Aprendes:

```
socket()
bind()
sendto()
recvfrom()
```

Conceptos:

- IP    
- puertos
- datagramas
- blocking I/O


---

## Proyecto 5 — TCP Chat System

Ahora:

```
listen()
accept()
recv()
send()
```

Aprendes:

- conexiones
- multiplexing (`select` / `poll`)
- múltiples clientes

Aquí entiendes cómo funciona Node, FastAPI o Go por debajo.

---

## Proyecto 6 — HTTP Server (🔥 clave)

Este proyecto cambia todo.

Implementa:

```
GET /
GET /users
```

Manual.

Aprendes:

- protocolo HTTP real
- parsing headers
- sockets persistentes
- buffers
- concurrencia básica

Después de esto, frameworks backend dejan de parecer magia.

---

# 🧠 Fase 4 — Sistemas reales (Semanas 13–18)

Ahora sí entran los proyectos pesados.

## SHA-512 Implementation

Aprendes:

- bitwise operations
- endianess
- optimización
- buffers binarios    

Esto fortalece pensamiento low-level brutalmente.

---

## Port Scanner + Ping

Aprendes:

- raw sockets    
- ICMP
- timeouts
- networking profundo

Empiezas a pensar como herramientas tipo `nmap`.

---

## FTP Server

Aquí juntas TODO:

- filesystem    
- networking
- protocolos
- parsing
- concurrencia

Este proyecto es prácticamente backend real sin framework.

---

# 🎮 Proyecto opcional — Asteroid Game

No es por diversión solamente.

Aprendes:

- event loops    
- memoria en tiempo real
- estructuras cache-friendly

El cerebro aprende performance sin darse cuenta.

---

# 🧩 Fase 5 — SQLite Integration

Ahora vuelves al primer proyecto:

👉 Notes Manager + SQLite

Aprendes:

- librerías C reales
- linking
- APIs externas
- manejo de errores profesional


Esto es backend real.

---

# 📚 Algoritmos y estructuras (en paralelo siempre)

Implementa en C:

- quicksort
- mergesort
- binary search tree
- heap
- trie
- LRU cache


Porque en C entiendes el costo REAL de cada operación.

---

# 🧭 Qué significa “backend usando C”

No vas a hacer CRUDs estilo Django.

Backend en C significa:

- servidores HTTP de alto rendimiento    
- proxies
- load balancers
- databases
- message brokers
- herramientas de red

Ejemplos reales escritos en C:

- nginx
- redis
- sqlite
- postgres (gran parte)
- linux kernel


---

# ⏱️ Timeline realista

Si estudias constante:

```
3 meses → cómodo con C
6 meses → sistemas funcionales
9–12 meses → nivel backend systems
```

El progreso no es lineal. Hay semanas donde todo crashea.

Eso es aprendizaje, no fracaso.

---

# 🧬 Regla de oro (la más importante)

Cuando algo falle:

1. compila con warnings máximos    
2. corre valgrind
3. usa gdb
4. NO adivines


C castiga la intuición vaga y recompensa el pensamiento científico.

---

# El cambio mental que va a ocurrir

Un día vas a abrir código Python o Go y pensar:

> “sé exactamente qué está pasando debajo”.

Ese día ya no estás aprendiendo C.  
Estás entendiendo computación.

Y curiosamente, ahí es cuando Rust empieza a tener mucho más sentido también.

El siguiente paso natural sería convertir esto en un **roadmap semanal extremadamente concreto (qué estudiar cada día + qué implementar)** para que avances sin parálisis por decisión.



### References


