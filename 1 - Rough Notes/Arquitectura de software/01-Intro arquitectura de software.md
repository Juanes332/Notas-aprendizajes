
2026-04-13 01:29

status: #child 

tags: [[Architectures]] [[Software Architectures]]


# Intro

La arquitectura de software representa las decisiones de design de nuestro sistema que afectan a su estructura, comportamiento y a la interacción de sus componentes internos. Es una **descripción a alto nivel de todo lo que comprende el sistema**

### **Requisitos Funcionales**

Se recogen todas las funcionalidades.

Pasos a seguir:

- **Identificar a los actores:**
	- Administrador, vendedor, comprador.

- **Recopilar casos de uso:**
	- Pago del contenido del carrito por parte de un comprador
	- Agregar un nuevo producto al catalogo por parte de un vendedor.

- **Diagramas de flujo:**
	- Interacciones entre los distintos componentes y actores en cada caso de uso

---
### **Requisitos no funcionales**

Factores que **mas impactan a la arquitectura** del sistema

- No es lo suficientemente **rápido**
- No es lo suficientemente **seguro**
- No soporta bien tantos **usuarios / peticiones al mismo tiempo.**
- No es sencillo **agregar o modificar funcionalidades**
- No es lo suficientemente **fiable**

Estos requisitos son también llamados **requisitos de calidad.** Los cuales deben ser medibles:

[x] "La web debe estar funcionando correctamente la gran mayoría del tiempo"
[x] "La web tiene que responder rápido a los usuarios"
[+] "La web debe responder en un tiempo inferior a los 300ms del 90% de las peticiones e inferior a 1s el 99% de las peticiones".

---
### **Restricciones**

**Técnicas:**
- Utilizar lenguaje o framework especifico
- APIs o servicios externos

**Legales**:
- Transacciones en el sector bancario
- Gobernanza de datos

**De negocio:**
- Presupuesto
- Tiempo
- Procesos internos

---

### **Importancia de elegir una buena arquitectura**

- Posibilidad de **escalar nuestro sistema** de unos pocos usuarios a **millones de usuarios concurrentes**
- **Facilidad de incluir nuevas funcionalidades** que a su vez escalen bien.
- Si no elegimos una buena arquitectura, **podemos perder meses de trabajo**.
- **Rediseñar una arquitectura es un trabajo muy costoso**, especialmente en sistemas a gran escala. Supondrá mucho **tiempo y dinero** a la empresa.

**Importancia del contexto**

Plataforma aparentemente sencilla (twitter/x)

- Publicar pequeños textos (tweets).
	- Pueden incluir imagen o video

- Acciones sobre los tweets
	- Like
	- Retweet / Repost
	- Comentar

- Posibilidad de seguir a otros usuarios


Escalar un sistema a millones de usuarios concurrentes no es sencillo **e implica solventar muchos problemas**. Ejemplo concurrencia a likes de un post de millones de personas



