---
layout: post
title: "Contando colaboraciones en Cuis Smalltalk"
date: 2021-07-29 20:00:00 -0300
comments: true
tags: metaprogramación
excerpt: Continuamos la serie de artículos sobre contar colaboraciones como una métrica de calidad de software. En esta ocasión, vamos a escribir un programa que cuente colaboraciones de un método en Cuis Smalltalk.
---

Continuamos la serie de artículos sobre contar colaboraciones como una métrica de calidad de software. Si no viste la
primer parte, te recomiendo leerla ya que allí explico de dónde sale esta métrica y por qué creo que es mejor que
contar líneas de código.

Recordemos brevemente a qué llamábamos colaboración: **envío de un mensaje a un objeto**. Entonces, lo que nos interesa
medir es cuántos envíos de mensaje aparecen en un método, ya que de esa manera podemos entender cuántas
responsabilidades o decisiones ocurren en un mismo lugar. Idealmente, queremos reducir ese número tanto como sea
posible.

Entonces, ¿podemos hacer un programa que cuente las colaboraciones de un método? ¡Sí! Aquí voy a comentar en detalle
la solución hecha en [Cuis Smalltalk](https://github.com/Cuis-Smalltalk/Cuis-Smalltalk-Dev), un ambiente Smalltalk de
código abierto y orientado a la simplicidad, del que tengo el orgullo de haber realizado algunas contribuciones.

¿Por qué esta elección de lenguaje? Al ser los ambientes Smalltalk reflexivos y metacirculares, es muy sencillo
manipular un programa de la misma manera que manipulamos, por ejemplo, una lista, una cuenta bancaria, o cualquier
objeto que se les ocurra. Y además contamos con herramientas que nos permiten visualizar estos objetos ¡y testearlos
como cualquier otro!

### Empezar por el principio: los tests

Vamos a hacer este contador con TDD, como no podía ser de otra manera. Y claro, al momento de escribir
[el primer test](https://blog.10pines.com/2020/08/18/el-primer-test/), y sobre todo en un metaprograma como éste, nos
puede resultar un poco difícil. Pensemos el caso más sencillo de alguien que quiera contar colaboraciones: ¡cuando no
hay ninguna! Vamos a crearnos una clase de test, que vamos a usarla para dos propósitos: escribir los tests
correspondientes a nuestro contador, y tener métodos de prueba con escenarios que nos sirvan para estos tests. Entonces
el primero de estos escenarios es un método vacío:

```smalltalk
CollaborationsCounterTest >> #emptyMethod
  "...nada por aquí..."
```

Y el primer test quedaría planteado de la siguiente manera:

```smalltalk
CollaborationsCounterTest >> #test01ItCountsZeroCollaborationsInAnEmptyMethod

	| counter |
	counter := CollaborationCounter for: self class >> #emptyMethod.

	self assert: 0 equals: counter value
```

Pequeña referencia para ubicarnos mejor en el mundo Smalltalk: el mensaje `>>` nos permite obtener un método compilado
(instancia de `CompiledMethod`) de la clase que recibe el mensaje (en este caso, la misma clase de test).

Luego de escribir el primer test... que nos guíen los [ZOMBIES](https://www.agilealliance.org/resources/sessions/test-driven-development-guided-by-zombies/).
Esta técnica, de James Greening, consiste en organizar tu estrategia de tests de manera de plantearlos en el siguiente
orden: `(Z)ero, (O)ne, (M)any, (B)oundaries, (I)nterfaces, (E)xceptional Behavior`. Es decir, empezar con el caso de
cero elementos que probablemente sea el más sencillo de hacer pasar, luego seguir con uno, con muchos, con casos borde,
luego con aquellos tests que terminen de definir nuestra interfaz con el mundo exterior, y por último el comportamiento
excepcional. El acrónimo cierra con la *"S"* de *"Simple solutions, simple scenarios"* que nos recuerda lo importante
de lo simple que nos conviene que sean los casos de prueba que vayamos escribiendo.

Así, los tests que fuimos construyendo paso a paso fueron los siguientes: (copio sólo los nombres para no redundar en
código de test que es muy similar al de `test01` pero con diferentes valores esperados):

```smalltalk
#test01ItCountsZeroCollaborationsInAnEmptyMethod
#test02ItCountsOneCollaborationInAMethodWithOnlyOneMessageSend
#test03ItCountsTwoCollaborationsThatArePlacedInDifferentsStatements
#test04ItCountsTwoCollaborationsThatArePlacedInTheSameMessageSend
#test05ItCountsThreeCollaborationsFromACascadeMessage
#test06ItCountsThreeCollaborationsInsideBlocks
```

### La implementación

La clave es poder tener un objeto que recorra el código de nuestro método bajo análisis e identifique cada vez que se
envíe un mensaje para sumar uno en un contador. Para ello, no vamos a recorrer el código fuente sino que vamos a
recorrer su representación en [Árbol de Sintaxis Abstracta](https://es.wikipedia.org/wiki/%C3%81rbol_de_sintaxis_abstracta)
(AST, por sus siglas en inglés) que es muchísimo más fácil de manipular. En Cuis, como en la mayoría de los sistemas
Smalltalk, tenemos un objeto `MethodNode` que representa un método en su totalidad y sería la raíz de nuestro árbol de
sintaxis. Luego vienen los diferentes elementos sintácticos como "hojas" de ese árbol (asignaciones, retornos, envíos
de mensajes, variables, bloques...). Y como cada tipo de nodo está asociado a una clase diferente, podemos gracias al
polimorfismo saber con precisión, por ejemplo, cuándo estamos en un `MessageNode` (nodo que representa envío de
mensaje).

Dijimos que íbamos a recorrer nuestro código, pero para ser estrictos, tampoco vamos a recorrer, sino que vamos
interactuar con otro objeto que lo haga por nosotros (*classic* orientación a objetos 🤣). Esta estructura de
`MethodNode` y sus nodos hijos necesita ser recorrida por varias tareas con fines diversos. Esto es un buen caso de uso
para el patrón [Visitor](https://refactoring.guru/es/design-patterns/visitor), que justamente propone la idea de objeto
"visitante" que sabe cómo ir recorriendo cada elemento de una estructura usando mensajes polimórficos para todos los
diferentes tipos de elementos con los que se puede encontrar.

Es por eso entonces que en Cuis tenemos al `ParseNodeVisitor`. Una clase abstracta que sabe cómo recorrer la estructura
de *parse nodes*, pero no hace nada. La idea es crear una subclase  que haga algo en los pasos en los que nos podemos
encontrar con envíos de mensajes. Es por eso que entonces nuestro `CollaborationCounter` redefine dos pasos del
`ParseNodeVisitor` en los que aparecen envíos de mensajes:

```smalltalk
CollaborationCounter >> #visitMessageNode: aMessageNode

	super visitMessageNode: aMessageNode.
	self countOneCollaboration

CollaborationCounter >> #visitMessageNodeInCascade: aMessageNode

	super visitMessageNodeInCascade: aMessageNode.
	self countOneCollaboration
```

Nótese el uso de `super` para continuar haciendo lo mismo que hace la superclase (`ParseNodeVisitor`) que sabe cómo
continuar la visita del árbol de sintaxis. La implementación de `#countOneCollaboration` es trivial, suma uno a una
variable de instancia inicializada en 0:

```smalltalk
CollaborationCounter >> #countOneCollaboration

	numberOfCollaborations := numberOfCollaborations + 1
```

Luego, la implementación de `#value` necesita iniciar este recorrido de nuestro objeto visitante y luego devolver el
resultado obtenido:

```smalltalk
CollaborationCounter >> #value

	methodToAnalyze methodNode accept: self.

	^ numberOfCollaborations
```

Dos cosas a tener en cuenta:

- `methodToAnalyze` es el método compilado que estamos analizando. Si le enviamos el mensaje `#methodNode` obtenemos
como resultado el `MethodNode` que representa nuestro árbol de sintaxis de nuestro método.
- `MethodNode` sabe "aceptar visitas" a través del mensaje `#accept:`. Aquí es donde invocamos al *visitor*.

### La demo!

Para que la herramienta sea más fácil de utilizar extendí el panel de "annotation" del Browser de Cuis para que cada
vez que estemos visualizando un método, podamos ver cuántas colaboraciones tiene. Así se ve!

![Contador de colaboraciones visualizado en el Browser de Cuis Smalltalk](/demos/collaborations-counter-cuis-smalltalk.gif "Contador de colaboraciones visualizado en el Browser de Cuis Smalltalk")

Todo el código y las instrucciones de instalación están acá: [https://github.com/ngarbezza/Cuis-Smalltalk-Utilities#collaborations-counter](https://github.com/ngarbezza/Cuis-Smalltalk-Utilities#collaborations-counter).

En las próximas ediciones, veremos cómo resolver esto mismo en otros lenguajes de programación.
