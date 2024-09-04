## FS CheckPoint 11

**Contenido:**

1. [¿Cuál es el comando para crear un repositorio git?](#cuál-es-el-comando-para-crear-un-repositorio-git)
1. [¿Por qué usamos ramas git?](#por-qué-usamos-ramas-git)
1. [¿Qué es un conflicto de fusión?](#qué-es-un-conflicto-de-fusión)
1. [¿Qué es el DOM?](#qué-es-el-dom)
1. [¿Qué es un oyente de eventos?](#qué-es-un-oyente-de-eventos)
1. [¿Qué es un selector de consultas?](#qué-es-un-selector-de-consultas)

## ¿Cuál es el comando para crear un repositorio git?

Asumiendo que lo tenemos instalado en nuestro sistema, para empezar a usar Git tenemos primero que crear un repositorio dentro de la carpeta que queramos usar para desarrollar el código. Para ello, entre otras opciones, podemos usar directamente la terminal. Una vez abierto el directorio donde queremos inicializar Git con la terminal, debemos usar el comando `git init`. Este comando va a crear una carpeta oculta *.git* que va a contener toda la información que va a ir almacenando Git. 

El ejemplo siguiente muestra la creación de un repositorio dentro de la carpeta *GitExample*, la cual contiene dos ficheros creados previamente por el desarrollador pero que podría estar inicialmente vacía. El ejemplo muestra el contenido de la carpeta creada tras la inicialización del repositorio.

![Git init example](/images/gitInit.png)

Una vez creado el repositorio, ya estamos en condiciones de empezar a decirle a Git que información debe tener en cuenta. Para ver si se están siguiendo los cambios en el código podemos usar el comando `git status`. Siguiendo con el ejemplo, la imagen siguiente nos muestra que los ficheros ”main.js” y “readme.md” creados no están incluidos en el seguimiento hasta que se usa el comando `git add *`. En este caso, el asterisco es un carácter comodín que indica que queremos añadir todos los ficheros del directorio excepto los que empiezan por punto pero podríamos añadirlos uno a uno. 

![Git Add example](/images/gitAdd.png "Ejemplo Git add")

Para guardar el estado del proyecto en desarrollo debemos indicarlo o ”acometerlo” usando el comando `git commit –m “mensaje”»`, donde “mensaje” representa una descripción de las novedades introducidas que nos permita seguir la evolución de los cambios realizados. Tradicionalmente, como se muestra en la imagen de abajo, el primer commit suele acompañarse de las palabras “initial commit” o “First commit”. 

![Git commit example](/images/gitCommit.png "Ejemplo de git commit")

Tras los pasos realizados hemos creado la rama principal del proyecto en nuestro ordenador, es decir, en nuestro repositorio local. Una práctica habitual y recomendable es crear un repositorio remoto para así poder trabajar de manera coordinada con otros desarrolladores. A modo de ejemplo se ha creado el repositorio remoto “GitExample_Remote” en GitHub. En la imagen siguiente se muestra el código necesario para indicar que queremos añadir (“add”) un repositorio remoto que se encuentra en la url indicada y al que queremos denominar “origin”. En el ejemplo, el comando `git branch -M main` indica que queremos que el repositorio local se denomine “main”. Una vez hecho esto, se usa `git push` para copiar el contenido del repositorio local al repositorio remoto. 

![Push to GitHub](/images/pushToRemote.png "Ejemplo git push a GitHub")


## ¿Por qué usamos ramas git?

El ejemplo desarrollado en el punto anterior nos permite introducir el concepto de “rama”. Conceptualmente, una rama es una bifurcación (ramificación) del estado del código que crea una evolución distinta a la de la ramificación principal. Las ramas nos permiten desarrollar características o funcionalidades (“feature branch”) nuevas antes de introducirlas en la ramificación principal. En la imagen siguiente, se muestra una estrategia de ramificación compuesta por la rama principal, denominada en este caso master (equivalente a la rama denominada main en el punto anterior) y varias “feature branch” secundarias. Esta estrategia nos permite trabajar juntamente con varios desarrolladores. 

![#Branching Strategy](/images/branching.webp "Ramificacion para desarollo")

Aun así, para equipos de desarrollo grandes, puede ser recomendable usar una estrategia de ramificación compuesta por una rama principal o estable junto a una rama de desarrollo de donde se realizan las ramificaciones funcionales. Cada vez que realizamos un `git commit`en la ramificación de desarrollo y el código funciona bien, pasamos también los cambios a la rama principal o estable, tal y como se muestra en la imagen:

![#Branching Strategy 2](/images/branching2.png "Ramificacion para desarollo en equipos grandes")

En Git, la creacion de ramificaciones se realiza con el comando `git branch newBranchName`, donde “newBranchName” hace referencia al nombre que queremos dar a la ramificación. Para consultar en que rama nos encontramos basta usar `git branch` sin especificar ningún nombre. Una vez creadas múltiples ramificaciones podemos usar el comando `git checkout branchName` para cambiar a o recuperar el estado del código en dicha ramificación. Otra alternativa es la de usar `git checkout –b NewBranchName` para simultáneamente crear y situarnos en una nueva ramificación. 

![#New branch creation](/images/BranchCreationExample.png "Ejemplo de creación de una rama")

Elaborando a partir del ejemplo desarrollado anteriormente, la imagen siguiente muestra el proceso de creación de una ramificación denominada “function-one” donde se crea un segundo archivo de código JS. Hay que tener presente que una vez desarrollada la funcionalidad deseada, esta pertenece únicamente a la rama empleada (en el ejemplo, function-one). 

![#Branch comparison example](/images/beforeGitMerge.png)

Tal y como se aprecia en la imagen, al volver a la rama principal, seguimos teniendo únicamente dos ficheros. Una vez testeada la funcionalidad, debemos mover el código a la ramificación principal usando el comando `git merge function-one` lo que fusionara los cambios realizados: 

![#Main branch after merge](/images/afterGitMerge.png)

Saliendo del entorno de desarrollo local, para subir la rama function-one al repositorio remoto (el cual previamente denominamos como origin) debemos escribir `git push –u origin function-one` tal y como se muestra en el ejemplo:

![#Git push to remote](/images/pushBranchToRemote.png)

En GitHub, el proceso de fusión es relativamente directo y visual puesto que se incluyen botones y comparaciones visuales para realizar el proceso de fusión (ver imagen siguiente).

 ![#GitHub merge](/images/mergeAndPullBranchGitHub.png)


## ¿Qué es un conflicto de fusión?

El uso de ramificaciones para trabajar conjuntamente con otros desarrolladores es una herramienta muy útil para el desarrollo colaborativo de aplicaciones. Durante este proceso es muy posible que dos desarrolladores realicen cambios en la misma línea de código. En estos escenarios se genera lo que se denomina un “conflicto de fusión” debido a que GitHub no tiene información para decidir que cambios son los correctos. Los conflictos de fusión ocurren típicamente al intentar fusionar nuestro repositorio local con el repositorio remoto puesto que este puede contener cambios realizados por otros desarrolladores. 

A modo de ejemplo, se ha simulado un cambio en el fichero “functionModule.js” del repositorio remoto y del local. Puesto que los cambios se encuentran en las mismas líneas, será necesario la realización de cambios en el proceso de fusión. Como se muestra en la imagen inferior, el proceso de fusión lo iniciamos con el comando `git fetch origin` el cual indica que queremos descargar (coger) el repositorio remoto remoto el cual habíamos denominado “origin”. Seguidamente, se realiza un commit donde se indica que el repositorio remoto y el nuestro difieren, pero todo parece normal. Seguidamente se inicia el proceso de fusión con la instrucción `git merge origin/main`, momento en el que se indica que el proceso automático de fusión ha fallado y que hay que resolver los conflictos y realizar un nuevo commit. 

![#Merge conflict example](/images/mergeConflict.png)

Para ver el conflicto debemos abrir el fichero “functionModule.js” generado por Git y que se muestra en la imagen que sigue:

![#Conflicted file example](/images/ConflictedModule.png)

En la imagen anterior, el código conflictivo que se encontraba en nuestro repositorio local se presenta primero después de la palabra “HEAD”. El código conflictivo del repositorio remoto (rama main del repositorio origin) se presenta a continuación. La resolución del conflicto corre a cargo del desarollador. 

Para finalizar, vale la pena resaltar que el ejemplo proporcionado usando las instrucciones `fetch` y `merge` sigue las practicas recomendadas. Este modo de proceder debe privilegiarse en frente al uso de las instrucciones `git pull origin`, que equivale a la realización de un `fetch` seguida de un `merge` de manera automática, lo que provoca que tengamos menos control sobre el proceso. 

## ¿Qué es el DOM?

Las siglas DOM significan “Document Object Model” se refieren a la estructura de un documento HTML, el cual esta formado por varias etiquetas (por ejemplo, head, div, …) que se encuentran anidadas unas dentro de las otras formando una estructura a modo de árbol, denominada *árbol DOM* (o solo DOM).

![#DOM scheme](/images/DOM.png)

Para poder trabajar con el DOM, JS dispone de un objeto denominado “document” el cual es una representación de la estructura de la pagina HTML que se encuentra en el navegador. Este objeto esta formado esencialmente por `ELEMENTS`, que hacen referencia a las distintas etiquetas HTML, y `NODES`, los cuales pueden representar Elements o nodos de texto. Además, document también contiene varios métodos que nos permiten trabajar con los elementos y los nodos. Destacaremos el método `document.querySelector()`, el qual permite seleccionar clases o IDs. Por ejemplo, `document.querySelector(“.myClass”);` seleccionaría el primer elemento de la clase “myClass”. 

## ¿Qué es un oyente de eventos?

Los eventos son señales de que algo ha ocurrido en un documento HTML como, por ejemplo, que se ha pulsado un botón o una tecla. Existen un gran número de eventos relacionados, por ejemplo, con acciones del raton, del teclado, entre otros. Entre los más populares podemos destacar `click` (pulsación del botón izquierdo del raton), `contextmenu` (pulsación del botón derecho del ratón), `mouseover/ mouseout` (cuando el cursor pasa por encima de un elemento o lo abandona), `mousedown / mouseup` (cuando el botón izquierdo es presionado o bien es soltado), `keydown / keyup` (cuando presionamos o soltamos una tecla del teclado).

Los oyentes de eventos son funciones JS que esperan que se produzca un evento para ejecutar el código indicado por el programador como, por ejemplo, una función que active o desactive una etiqueta(s) HTML del documento. Principalmente, existen tres maneras de crear oyentes de eventos: 

1.	*Atributos globales onevent de HTML*. HTML posee unos atributos globales llamados onevent (p.ej., `onclick`, `onmouseup`) que se pueden colocar directamente en el momento de escribir el código HTML de la página. El código siguiente muestra un ejemplo del uso del atributo `onclick` dentro de una etiqueta input. Al hacer click sobre el boton creado por la etiqueta se ejecuta la function `myfunction()`.

``` html
<!DOCTYPE html>
<html lang='en'>
<head>
  <meta charset='UTF-8'>
  <title>Event listeners</title>
</head>
<body>
    <input value="Log in" onclick="myfunction()" type="button">
</body>

<script>
    function myfunction(){
        confirm("You just clicked the button");
    }
</script>

</html>
```

2.	*Librería JQuery*. JQuery es una librería de JS muy popular que incluye métodos que escuchan distintos tipos de eventos. Algunos ejemplos de metodos JQuery son `.mouseover()` o `.load()`. Usando esta librería mediante CDN, el ejemplo anterior se convertiría en:

``` html
<!DOCTYPE html>
<html lang='en'>
<head>
  <meta charset='UTF-8'>
  <title>JQuery Event listeners</title>
  <script src="https://code.jquery.com/jquery-3.7.1.js"></script>
</head>
<body>
    <input value="Log in" class='myButton' type="button">
</body>

<script>
    $(".myButton").click(function(){
        confirm("You just clicked the button");
    });

</script>

</html>
```
3.	*Método JS `addEventListener()` del objeto document*. Si lo que queremos es usar JS nativo, debemos usar el método `.addEventListener()`. El código correspondiente a esta alternativa seria el siguiente:

```html
<!DOCTYPE html>
<html lang='en'>
<head>
  <meta charset='UTF-8'>
  <title>JS Event listeners</title>
</head>
<body>
    <input value="Log in" class='myButton' type="button">
</body>

<script>
    const myButton = document.querySelector(".myButton");

    function myFunction(){
        confirm("You just clicked the button");
    }
    
    myButton.addEventListener("click", myFunction);
</script>

</html>
```

Si nos centramos en buenas prácticas de programación, es preferible evitar el uso de los atributos onevent debido a que dificulta el mantenimiento de los documentos HTML. Usando una opción basada en programación permite cambiar o actualizar de manera mucho más rápida y fácil, manteniendo un código HTML más limpio. La elección entre una alternativa JS nativa o basada en una librería externa dependerá ampliamente de la experiencia previa y conocimientos del programador. 

## ¿Qué es un selector de consultas?

Los selectores de consultas son métodos del objeto JS `document` que permiten seleccionar elementos HTML, gráficos o fórmulas matemáticas de un documento HTML. La selección del elemento(s) se basa en los selectores CSS que se especifican. Si no se encuentra ningún elemento, estos métodos devuelven `null`. Existen dos métodos selectores: `.querySelector(selectors)` y `.querySelectorAll(selectors)`, donde selectors hace referencia a una cadena de caracteres que contiene uno o varios selectores CSS. El código siguiente demuestra el uso de estos dos métodos. Como se aprecia, `querySelector(“.coolDiv”)` recupera el primer objeto de clase “coolDiv”. Se demuestra también la combinación de varios selectores CSS con la string “.coolDiv#nice”. Finalmente, se muestra la selección de varios objetos con el método `.querySelectorAll()`.

```html
<!DOCTYPE html>
<html lang='en'>
<head>
  <meta charset='UTF-8'>
  <title>JS Query Selectors</title>
</head>
<body>
    <div class="coolDiv" id="notNice">div one</div>
    <div class="coolDiv" id="nice">div two</div>
    <div class="coolDiv">div three</div>
     
</body>

<script>
    const firstCoolDiv = document.querySelector(".coolDiv");
    console.log(firstCoolDiv) //? <div class="coolDiv" id="notNice">div one</div>
    const coolAndNiceDiv = document.querySelector(".coolDiv#nice");
    console.log(coolAndNiceDiv); //?<div class="coolDiv" id="nice">div one</div>
    const allCoolDiv = document.querySelectorAll(".coolDiv"); 
    console.log(allCoolDiv); //? NodeList(3) [div#notNice.coolDiv, div#nice.coolDiv, div.coolDiv]
</script>
</html>
```
