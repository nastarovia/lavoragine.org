---
layout: post
title: 'Edición sostenible con pandoc, markdown y git. Parte 2: Objetivo, herramientas & requisitos'
date: 2020-08-22 00:00:00 -0000
tags: [edicion digital, Proyectos]
image: flower2.jpeg
---
El propósito de este tutorial será mostrar desde un punto de vista práctico como combinar herramientas para crear un libro multiformato, de una mera sostenible y automatizada, y que, de paso, resuelve punto por punto nuestro problema..

Este es únicamente un posible *workflow* y desde luego que existen muchas otras herramientas y combinaciones. El punto que espero demostrar es únicamente la cantidad de herramientas que existen a nuestra disposición, que pueden transformar el proceso de producción de un libro hasta el punto de permitirnos automatizar todas las tareas, ganando en eficiencia y al mismo tiempo en control sobre nuestros proyectos.

Para mostrar esto, voy a replantear mi proyecto (la edición de *Mío cid campeador* de Vicente Huidobro) y rehacerlo desde el principio. Para ello:

- Crearemos, a partir de los archivos que tenemos, un set de archivos fuente en markDown (texto plano con la extensión ``.md``)
- Luego crearemos un repositorio para albergar los archivos necesarios
- Crearemos un archivo formateado en ``yaml`` para almacenar la metadata del libro
- Pondremos este repositorio (el directorio donde vive nuestro proyecto) bajo control de versiones con ``git``
- Sincronizaremos este repositorio local con un repositorio remoto en GitHub
- Utilizando pandoc, crearemos nuestro Epub, y crearemos un archivo ICML para importarlo en inDesign
- Crearemos desde InDesign el PDF final de imprenta
- Veremos como hacer correcciones, manteniendo un único set de archivos fuente y guardando un registro histórico de correcciones.
- y por último, crearemos una página web con los archivos de nuestro libro.



### 2.1 Requisitos

Para seguir este tutorial, es aconsejable (aunque no indispensable):

- Alguna familiaridad con la línea de comandos.
- Tener conocimientos básicos de HTML y CSS.
- Un conocimientos de InDesign a nivel de diseño editorial.
- Una mente abierta (todo lo anterior se puede reemplazar con una aplicada búsqueda en Google, esto último no).

### 2.3 Herramientas & software

Y deberemos tener instalado en nuestro PC:

- Un editor de texto plano (no procesador de texto): yo utilizaré Atom, pero hay una serie de editores de texto igualmente útiles y libres (Visual Studio Code, Sublime Text, Vim, etc...)
- Línea de comandos (en Windows, el PowerShell; en Linux, el terminal o uno cualquiera de sus emuladores; en Mac, la terminal).
- Pandoc
- InDesign
- Git y el cliente de escritorio de GitHub
- MS Word
- Jeckyll

Instalar software es tedioso. De momento, vamos a sumir que (como yo) tenéis todo el software instalado en vuestro ordenador.

Y dado que de lo que se trata es de probar algo distinto (no se puede esperar un resultado diferente si hacemos lo mismo), vamos a cambiar nuestro enfoque y utilizar Markdown como formato base, en lugar del consabido MS Word. Las razones para escoger markdown son exactamente las mismas que para desechar MS Word y se pueden resumir en los tres mantras de cualquier workflow editorial en un entorno digital:

- estructura
- sostenibilidad
- interoperatividad

Necesitamos un archivo bien estructurado. Esto garantiza que pueda ser mapeado correctamente para la importación en InDesign, que se pueda generar un epub automáticamente, y que este epub, tal cual ha sido exportado por pandoc, sea accesible.

El hecho de que trabajemos en texto plano (markdown más que un formato, es una convención para etiquetar texto plano), garantiza que estos archivos sean legibles por cualquier ordenador, sea cual sea el sistema operativo y el software que tengan instalado, ahora, hace treinta años o en el siglo XXII. Además, son tan interpretables por máquinas como legibles por humanos, y pesan ridículamente poco comparados con sus colegas con ínfulas, los archivos generados por procesadores de texto como MS Word o Open Office.

Y por estas razones, son también fácilmente trasladables (de una maquina a otra, de un software a otro, de un lenguaje de etiquetado a otro). Si el archivo está bien estructurado, podemos hacer en efecto lo que queramos con él.

Manos a la obra.


### Configurar el espacio de trabajo

Lo primero será crear el directorio donde van a vivir los archivos de mi proyecto. En mi ordenador será la carpeta ``mioCidCampeador``. Dentro de esta carpeta, además crearé una carpeta para las imágenes, otra para los archivos markdown y una última que llamaré ``src`` (por "source": el archivo fuente o archivo base). para ello, nos situamos con el terminal en el directorio donde queremos crear la carpeta de nuestro proyecto), y escribimos:

````
mkdir mioCidCampeador
cd mioCidCampeador
mkdir imagenes, markdown, src, indd, icml,
````

Naturalmente, también se puede crear la carpeta haciendo ``click derecho > crear carpeta``, pero para los propósitos de este tutorial, utilizaremos la línea de comandos: es más rápido y de esta manera nos familiarizamos con su uso.

Una vez creada nuestra carpeta, podemos poner nuestro documento (si: mioCidFinal.docx) en la carpeta correspondiente (``src``). Este es el punto de partida de nuestro proyecto.
