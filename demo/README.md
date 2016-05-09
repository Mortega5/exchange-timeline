# Demo de usabilidad (en desarrollo)

El propósito del código contenido en el fichero `index.html` es medir ciertos atributos y caracteristicas de los diferentes componentes web (en este caso el de SFO) basando en 
ciertos `items` de la lista del W3C

Todos los items no son elementos implementables en los componentes web, ya que no pueden ser automatizados de una forma sencilla.
Se han implementado los siguientes items

  * (1.4.7) Todos los elementos de audio pueden ser controlados por el usuario
  * (2) Todo el texto tiene un ancho máximo de 80 caracteres (en desarrollo)

La estructura que se ha seguido para establer cómo se evaluarán los diferentes elementos, es una estructura de Objeto, similar a un JSON. Cada clave define un valor, donde el valor
es la función autocontenida que devuelve si se cumple o no el item que se quiere medir. A causa de que ciertos elementos se miden mediante funciones asíncronas se ha de pasar una función
`callback`, es decir, una función que se ejecutará una vez que se comprueben todos las condiciones y que recibirá como parámetro un booleano indicando
si se cumple o no dicha métrica.

## Especificación de como funciona

Lo primero que se realiza es obtener todos los elementos que existen en el DOM. Para asegurarse que todos los elementos se han cargado correctamente
se ha establecido un tiempo de espera hasta que se presupone que se han cargado todos los elementos.

Una vez que se han cargado todos, lo primero que hacemos es obtener todos los elementos existentes en el DOM y se analizán. Es importante destacar
que por la naturaleza de los componentes, presuponemos que los contenidos que se se pueden obtener del componente son todos los elementos visibles por el usuario
lo que implica lo siguiente:
  
  * No se cargará contenido de manera dinamica mediante javascript.
  * Se asumiran que los `templates` que estan ocultos no forman parte de la parte medible del componente.
  
Esta última limitación es bastante estricta y poco realista pero debido a las simplificaciones llevadas a cabo en el componente
es necesario realizarla. La primera hipotesis, es algo que se asume que se realizará puesto que es una buena practica por parte de un
desarrollador separar la vista del controlador (Modelo Vista-Controlador (MVC))

La función que se emplea para obtener todos los elementos del dom es `getElementByTagName` por motivos de eficiencia.
Una vez que se obtienen todos los elementos se realizan las métricas.

  * *1.4.7*
  Como vamos a medir los audios, hay que destacar que se va a realizar a través de los eventos de `onPause` y `onPlay` nativos de los propios
  elementos. Para asignar esta funcion de callback tomaremos todos los elementos existentes cuya etiqueta es audio. Para evitar sobreescrituras
  de los metodos de callback especificados para cada elemento, se guarda en un array las funciones de callback predefinidas, para posteriormente
  volverselas a asignar. (linea 54 a la 58)
  
  Una vez tenemos todos los elementos y todas las funciones de callback asignadas, recorreremos todos los elementos del dom
  para realizar click en aquellos que son clickables y no son etiquetas de tipo `a` (esto se realiza para evitar redireccionamientos a otros lados)
  Cabe destacar que se realizan una doble iteración por todos los elementos del DOM para evitar que si un elemento cambia su naturaleza cuando
  es cliclado pueda ser registrado en ambos estados. (creo que hay que mejorar en que se debe ejecutar su función nativa, tengo que verlo)
  Linea (60 a 64)
  
  Una vez establecido todo esto, se estable un setTimeout para recoger todos los callback de los elementos y se va guardando su etado en una variable
  En caso de no tener los dos estados activados a 1 por la función de callback, significa que los elementos no han sido pausados o no se han ejecutado (play)

  * 2
    
  Esta métrica se encuentra en desarrollo, pero se basa en medir el ancho con de un caracter con las mismas caracteristicas de un texto encontrado (tamaño, estilo y el ancho de la linea)
  Una vez que tenemos el ancho de un caracter con la mismas caracteristicas basta con multiplar por 80 (medida que establece la metrica)
  y ver si tiene más o menos dimension que el texto a medir.


