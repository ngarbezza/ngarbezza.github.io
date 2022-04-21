---
layout: post
title: "Los defaults malvados"
date: 2022-04-19 12:00:00 -0300
comments: true
tags: diseño
excerpt: "Trabajar en un proyecto con mucho código heredado siempre es una fuente de grandes aprendizajes. En esta ocasión les
presento a los \"defaults malvados\", como a mí me gustan llamarlos. Son partes de código que pueden hacer mucho daño
en una aplicación. Veamos cómo..."
---

(traducción al castellano de _The Evil Defaults_, para ver su versión original
[ir aquí](https://blog.10pines.com/2018/09/27/the-evil-defaults/))

## Intro

Trabajar en un proyecto con mucho código heredado siempre es una fuente de grandes aprendizajes. En esta ocasión les
presento a los _"defaults malvados"_, como a mí me gustan llamarlos. Son partes de código que pueden hacer mucho daño
en una aplicación. Veamos cómo...

## ¿Qué sería un default malvado?

Un **default malvado** es una pieza de código que actúa, como su nombre lo indica, como una opción **por defecto** en un
flujo de decisión y que bajo ciertas circunstancias **causa problemas**, relacionados al comportamiento esperado
**a nivel negocio**, cuando se evalúa.

Cuando digo pieza de código me refiero a un simple objeto, valor, o a un conjunto de acciones con efectos, o incluso a
la deliberada decisión de no hacer nada. Hacer nada también es hacer algo...

Abstractamente hablando:

```ruby
if regla_de_negocio_a_aplica
  hacer_a
elsif regla_de_negocio_b_aplica
  hacer_b
else
  # a veces no sabemos qué hacer aquí, entonces… ¯\_(ツ)_/¯
  hacer_algo_que_parecería_estar_bien
end
```

[No nos llevamos muy bien con los condicionales](https://blog.10pines.com/2014/10/20/trust-in-objects-they-have-the-right-to-decide/),
pero este simple pseudocódigo es sólo para mostrar el potencial problema :-) Preferimos objetos polimórficos que se
encarguen de resolver estas decisiones. En cualquier caso, sea una rama de un condicional or un objeto polimórfico que
representa ese caso, estamos tomando decisiones. Y debemos elegir cuidadosamente qué hacer en la rama del `else`, porque
es también un flujo que puede aplicar en muchos casos, algunos de ellos quizás que no imaginamos al momento de construir
el programa.

El default malvado se vuelve más malvado a medida que continuamos con la ejecución, porque una mala decisión (que no
se condice con lo que a nivel negocio esperamos que suceda) puede poner nuestro sistema en un estado inconsistente o
arrastrar un error a otras partes del programa. A menos que lancemos una excepción, el programa continuará haciendo
cosas en base a lo que ese default haya determinado.

¿Qué problemas tenemos entonces?

* El sistema parece funcionar, porque "no se rompe". Pero funciona de una manera incorrecta. Hay una falsa sensación de
éxito.
* Cuando descubirmos que había un error, probablemente ya sea demasiado tarde. Depurar y corregir estos errores suelen
llevar mucho tiempo por los efectos que dejan, y porque a veces no tenemos alguna excepción para analizar.
* El código no está reflejando lo que necesita ser modelado. Si no sabemos qué hacer en estos casos (¡y puede que el
cliente a nivel negocio tampoco lo sepa!), lo mejor que podemos hacer es
[fallar con una excepción](https://blog.10pines.com/2014/09/29/symbols-the-new-return-codes-part-2/).

## ¿Por qué ocurren?

Les programadores hacemos una observación del dominio y luego intentamos reflejarla en un modelo computable. Y sabemos
que en ese proceso seguramente no tengamos una imagen perfecta. Necesitamos de iteraciones, y a medida que avancemos
ganaremos experiencia y conocimiento en el dominio que estamos modelando. Mientras tanto, tenemos que tomar muchas
decisiones!. Para la computadora no hay grises, debemos ser precisos incluso en contextos de incertidumbre cuando aún
no entendimos el problema a modelar en su totalidad...

Es así que la falta de conocimiento en el dominio es la razón principal para tener defaults malvados. Cuando los
introducimos, no nos damos cuenta que algo puede salir mal. Pensamos que funciona. Al menos hoy.

Otra razón para tener defaults malvados es la sobresimplificación del problema que estamos modelando. A veces pensamos
que un simple valor o línea de código puede salvarnos de cualquier posible casi, para luego darnos cuenta más tarde que
nos habíamos equivocado.

Además, tenemos una variable más que es el "miedo a fallar" que a veces experimentamos como desarrolladores. Simplemente
parece mejor "hacer algo" o "devolver algo" en lugar de fallar, porque lo vemos como algo problemático. Pero la realidad
es que si no sabemos qué hacer, es mejor no asumir y fallar, eso reflejará la realidad y nos dirá que debemos hacernos
cargo de esa incertidumbre si ocurre.

## Un ejemplo de la vida real

Estábamos junto con mi equipo realizando unas mejoras en un proceso automatizado que importaba compras hechas en Ebay y
Amazon en un ecommerce a través de APIs REST. Entonces, teníamos un endpoint al que consultábamos las compras
realizadas, las traducíamos en el formato que nuestro ecommerce esperaba para guardarlas y utilizarlas más adelante.
Las compras tenían como uno de sus datos el país del comprador que para nuestro sistema era un dato indispensable para
el proceso de fulfillment (hacer que los ítems lleguen a las manos de los compradores).

Para poder guardar correctamente el país necesitábamos hacer un mapeo desde el formato de estos marketplaces externos
al nuestro. Por ejemplo. "ARG" mapea a "Argentina". Bueno, esta lógica tenía un default malvado: se elegía USA (Estados
Unidos) como la opción por defecto en caso de no encontrar una entrada existente en el mapeo. Teníamos opciones muy
limitadas de envío con lo cual esto no fue un problema por varios meses... hasta que sucedió. Y cuando sucedió, no fue
inmediato darnos cuenta. Nada explotaba. Sólo continuaba con un proceso que iba a terminar mal por haber elegido el país
incorrecto. Los clientes comenzaron a enviar reclamos porque sus compras no llegaban a tiempo. No fue sencillo encontrar
la causa del problema, la solución fue sencilla pero resolver los casos que ya habían ocurrido (sumado a los negativos
comentarios de los clientes afectados) fue difícil.

## ¿Y cómo lo resolvemos?

Como mencionamos anteriormente, una manera apropiada de modelar un caso desconocido es con una excepción. Detener el
flujo de ejecución y esperar por intervención humana para ver ese error y decidir qué hacer con ello.

Una cosa clave es tener algún mecanismo para saber cuándo ocurren estas excepciones, y tener toda la información
necesaria para investigar el problema. Necesitamos:

* Buenos mensajes de error, descriptivos y que contengan información adicional del contexto en el que este error ocurrió
(IDs/números de referencia, valores de configuración, si es una request web, sus parámetros y cuerpo).
* Un lugar en donde se reporten estos errores. Recomiendo un servicio de agregación de errores como
[Rollbar](https://rollbar.com/) o [Honeybadger](https://www.honeybadger.io/). Y si está conectado a un sistema de
alertas, mejor aún.
* Alguien responsable de accionar cuando estos errores ocurren. Por ejemplo, algo que se puede implementar en un equipo
es un rol de _triager_, en donde una persona del equipo, que va rotando por día o semana, tiene la responsabilidad de
ver los errores que están ocurriendo, analizar su gravedad y reportarlos al lugar correcto o ignorarlos si no
representan un problema real.

## ¿Y existen defaults buenos?

¡Por supuesto! Hay una discusión interesante en [este hilo de StackExchange](https://softwareengineering.stackexchange.com/questions/63908/default-values-are-they-good-or-evil)
que abarca precisamente este tema. Una de las respuestas presenta un muy buen ejemplo: sabemos que ciertos protocolos
tienen un puerto por defecto asociado, que en el caso de FTP es el puerto 23. Entonces, cuando abrimos una conexión FTP
sin especificar un puerto, es seguro asumir que el 23 va a ser el puerto por defecto. Es algo universalmente conocido y
documentado. En definitva, algo que el dominio nos enseñó.

## Conclusiones

* Pensemos antes de escribir un _default_. Preguntémonos lo siguiente: estamos seguros que todos estos casos no
contemplados caigan en esta lógica? Cuáles son los posibles bugs que estamos ocultando? Estamos sobresimplificando el
problema?
* Los _defaults_ deben, al igual que cualquier parte del código, tener tests automatizados. De esta menera, tendríamos
una explicación de al menos un caso que nos motivó a introducir este _default_. Si describimos (a través de un comentario
o en el nombre del test) por qué introducimos esta lógica, puede ayudar a entender este código en el futuro. De esta
manera, podemos continuar haciendo TDD e iterando en caso que necesitemos cambiar esta lógica relacionada al _default_.
* Fallemos rápido. El ciclo de feedback se acorta drásticamente y nos damos cuenta pronto que debemos accionar. No
temamos lanzar errores. Sólo preocupémonos porque el mensaje de error sea lo suficientemente descriptivo para que
podamos accionar.
