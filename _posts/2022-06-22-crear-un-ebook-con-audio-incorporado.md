---
layout: post
title: 'Crear un ebook con audio incorporado'
date: 2022-06-15 
tags: [Edicion digital, Audiolibros, Proyectos, Epub3]
image: leaves.jpg
excerpt_separator: <!--more-->
---

Mi objetivo era añadir a la versión en epub del libro "La misteriosa maternidad del verso" (que recoge tres conferencias de Gabriela Mistral, Juana de Ibarbourou, y Alfonsina Storni) los audios existentes de las grabaciones originales de las conferencias.  La calidad de las grabaciones es miserable, pero como documento tienen un valor. Son emocionantes, vaya. La cuestión es cómo incorporarlas en un libro electrónico.

Vamos a ello.

Mi workflow habitual es crear los libros electrónicos partiendo del texto formateado en  Markdown. Markdown es un formato de marcado ligero (o mejor: una convención de marcado), que permite escribir HTML utilizando marcas muy fáciles de aprender. Si combinamos los archivos en markdown con la metadata formateada en YAML (que es un formato de serialización de datos, que nos permite tabular información sobre los libros para utilizar en otras herramientas) y con Pandoc para realizar la conversión, tenemos que con una instrucción podemos generar un libro electrónico en formato Epub3. 

El primer problema es que markdown no incluye en su sintaxis un procedimiento para marcar archivos de audio. Una opción es marcarlos como links que vinculen a una carpeta con los audios en mp3. Pero no funciona. Cuando convertimos el markdown en epub, el link está correctamente escrito, pero no hay nada en la carpeta `audio` :

![exportar audio con Pandoc](/assets/img/audio_export_via_pandoc.png)

Una forma de resolver el problema es renunciar a añadir el audio en este paso y agregarlo al Epub una vez lo hallamos convertido utilizando Sigil. En este caso lo que hice fueron cinco cosas: 

- Abrir el Epub generado con  `pandoc` en sigil
- Añadir los archivos de audio (en MP3) directamente en la carpeta Audio:  `add existing files` (`ctrl + click` en mac, click derecho en un PC),  seleccionar los archivos y aceptar. Veremos cómo en la carpeta de audio de sigil tenemos los tres archivos.

![carpetas en sigil](/assets/img/sigil_audio_folder.png)

- Crear un archivo `playlist.xhtml` para listar las grabaciones, con la siguiente estructura:

	````
	<body>
	<section epub:type="backmatter loa">
	
	<!–– el `epub-type="loa"` significa "List of Audios" y es lo que va al caso.-->
	
	<h1 class="playlist">Playlist</h1>
	
	<!–– Aquí creamos el contenedor para el primer track-->
	
	<div class="playlist">
	<hr class="sepBar"/>
	<table class="playlist">
    <tbody><tr class="playlist" id="track01">
        <td class="playlist">
            <div class="playlist">
            <p class="playlist">
            <span class="trackNumber">01 </span>
            <span class="trackTitle"><a href="ch002.xhtml">Casi en pantuflas / Juana de Ibarbourou</a></span>
            <span class="trackDuration">(22' 58")</span>
            </p>
            <audio class="playlist" src="../audio/juana_de_ibarbourou.mp3" controls="controls">Tu aparato no admite audio</audio>
            
	<!–– Aquí, cuidado: hay un bug en sigil que hace que insertando el  link al archivo de sonido con el menu `insert > file` ponga `../Audio` con minúsculas cuando debe ir en minúsculas -->
	 
            </div>
        </td>
    </tr>
    <tr class="playlist">
        <td class="playlist">
            <span class="trackTitle">Duración total: 22' 58"</span>
        </td>
    </tr>
    ````
- Y luego, crear links en los capítulos correspondientes a cada una de las conferencias vinculados con la lista de archivos de audio.

````
<p class="nav_bar"><a href="playlist.xhtml#track01">[Escuchar]</a></p>
````

- Por último, es buena idea añadir la metadata pertinente. Para eso, hay que abrir el archivo `content.opf` y escribir (antes del cierre de la etiqueta `</metadata>`), la metadata relativa a accesibilidad:
````
	<meta property="schema:accessibilityFeature">alternativeText</meta>
    <meta property="schema:accessibilityFeature">readingOrder</meta>
    <meta property="schema:accessibilityFeature">structuralNavigation</meta>
    <meta property="schema:accessibilityFeature">tableOfContents</meta>
    <meta property="schema:accessibilitySummary">The publication has been evaluated 
     for accessibility.</meta>
    <meta property="schema:accessibilityHazard">none</meta>
    <meta property="schema:accessMode">textual</meta>
    <meta property="schema:accessMode">visual</meta>
    <meta property="schema:accessMode">auditory</meta>
    <meta property="schema:accessModeSufficient">textual</meta>
    <meta property="schema:accessibilityControl">fullKeyboardControl</meta>
    <meta property="schema:accessibilityControl">fullMouseControl</meta>
````

Una vez añadidos y vinculados todos los audios, es una buena idea validar el archivo. Primero con  [epub-check](https://www.w3.org/publishing/epubcheck/) , el validador de archivos Epub3 del W3C. Y luego con [Ace](https://daisy.org/activities/software/ace/), el validador dedicado a evaluar la accesibilidad de los archivos epub. En este caso, me arrojó un montón de advertencias que en su mayoría tenían que ver con la ausencia de `aria-roles`, lo que se arregla con un poco de regex (a cada `epub type` le corresponde un `aria role`: la lista de equivalencias se puede consultar [aquí](https://idpf.github.io/epub-guides/epub-aria-authoring/)). Un ejemplo:

buscar `epub:type="footnote"`
y reemplazar por: `epub:type="noteref" role="doc-footnote"`

Y así hasta arreglar cada una de las advertencias. 

![Ace: aplicación para evaluar la accesibilidad en archivos Epub](/assets/img/ace_output.png)

Con esto, tenemos un `epub` con texto y audio vinculado, accesible y listo para usar. 
Se puede descargar una muestra del libro [aquí](https://github.com/lavoragine/la_misteriosa_maternidad_del_verso/releases/tag/demo).







