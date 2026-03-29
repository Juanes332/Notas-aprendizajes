
2026-03-28 18:35

status: #baby 

tags: [[Spanish]] [[English]] [[Cloud]] [[AWS]] [[DevOps]]


# IAM

IAM son las siglas de Identity and Access Management, es un servicio global y nos permite gestionar los usuarios y grupos de nuestra cuenta de AWS.

Cuando creamos una cuenta de AWS se crea el usuario root, este usuario root por buenas practicas jamas debe ser usado para el despliegue de servicios en la consola de AWS ni debe ser compartido. Para ello se crean usuarios a los cuales les podemos asignar permisos especificos en la consola de aws, lo mismo para los grupos. Algo importante a tener en cuenta es que  los usuarios pueden estar en en grupos o en solitario (es decir sin grupo) pero los grupos solo pueden contener usuarios y no otros grupos; esto es basicamente porque varios usuarios pueden estar en varios grupos a la vez y tener permisos operativos de manera individual y permisos heredados por uno o varios grupos. Por ejemplo puede estar el grupo de devs, grupo de operaciones y grupo de redes, puede haber un dev que pertenezca en paralelo al grupo de operaciones y uno de red que pertenezca al de los devs.

Los usuarios o los grupos pueden tener permisos por medio de las policies, las cuales son las reglas que nos permiten manejar de manera mas comoda estos permisos. Normalmente estas policies se manejan en documentos JSON como este:

![[Pasted image 20260328185419.png]]

Estas politicas definen los permisos de los usuarios y grupos siguiendo el principio de least privilege, lo que se resume a que no debemos darle mas permisos a un usuario de lo que realmente necesita, solo lo necesesario.

Al momento de crear un usuario podemos descargar el .csv con las credenciales parciales, estas credenciales el usuario creado podrá cambiarlas por unas propias al momento de hacer login.

![[Pasted image 20260329164714.png]]

**Ejemplo de como se ven las creds .csv**

User name,Password,Console sign-in URL
jenhz,wVf04N4!,https://252632662295.signin.aws.amazon.com/console


**Herencia de políticas**
![[Pasted image 20260329165403.png]]

Varios usuarios pueden estar en diferentes grupos y compartir políticas de la consola de aws, lo que nos permite aws es que estos usuarios pueden estar en diferentes grupos y heredar políticas.


**Estructura de politicas**
![[Pasted image 20260329165546.png]]



