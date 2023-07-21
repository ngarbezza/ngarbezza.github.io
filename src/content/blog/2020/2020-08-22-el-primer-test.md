---
title: El Primer Test
author: Nahuel Garbezza
pubDatetime: 2020-08-22T12:00:00
postSlug: el-primer-test
canonicalUrl: https://blog.10pines.com/2020/08/18/el-primer-test/
featured: false
draft: false
tags:
  - tdd
  - testing
description: "Quizás la mayor dificultad al trabajar con TDD es empezar. ¿Cómo ir más allá de la hoja en blanco? TDD nos invita a aprender muy rápido, a sabiendas que nos vamos a equivocar mucho. ¡a prepararse, pues! ¿Qué características debería tener ese difícil primer test?"
---

## Intro

Quizás la mayor dificultad al trabajar con TDD es **empezar**. ¿Cómo ir más allá de la hoja en blanco?
¿Cómo resistir a la tentación de _armar un sistemita_ en nuestra cabeza o en una hoja de papel?
¿Cómo tener la certeza de que sabemos bien lo que vamos a hacer y empezar _a todo trapo_?

Muchas veces las personas queremos aprender, pero no queremos quedar en ridículo mientras lo hacemos.
Aún está instalada esa idea de "hacerlo bien la primera vez" o "que si me equivoco, que no se note",
incluso en el software, donde es bien sabido que manejamos altos niveles de incertidumbre. TDD viene a
romper con eso, y nos invita a aprender muy rápido, a sabiendas que nos vamos a equivocar mucho.
¡a prepararse, pues!

¿Qué características debería tener ese difícil primer test? Veamos...

## Lo más barato posible

En el desarrollo de software el tiempo es crucial. Todo cuesta... tiempo y dinero. Entonces, si nos
vamos a equivocar y darnos cuenta que era por otro camino, qué mejor que eso ocurra dentro del loop
de una sesión de TDD, que no debería durar más que unos minutos. No hay nada peor que invertir horas,
días y semanas para darnos cuenta de que aquello que construimos con mucho empeño no era lo que se esperaba.

## Lo más rápido e incorrecto posible

Sin feedback inmediato, no hay TDD. Si nos sentamos media hora enfrente de la computadora sin escribir
un test, es tiempo que perdimos en saber si lo que estamos haciendo va en una dirección correcta o no.
Por más que no tengamos mucha idea de cómo se va a ver la solución (¡es lo normal!) es importante escribir
algo. Un primer _assert_ que nos permita pensar en cuál va a ser el próximo. O al menos algo que nos permita
una contradicción.

## Ni siquiera un nombre

Dada la incertidumbre inicial, lo que más nos va a costar es ponerle un nombre _bonito_ al test. ¿Pero qué
importa el nombre cuando estamos en t=0? Poco y nada. El nombre no se ejecuta, y si bien sirve para futuras
lecturas de programadores... para eso está el paso de refactoring. Pongamos `testXXXXX`, y sigamos. En algún
momento ese `XXXXX` nos va a molestar y vamos a estar en condiciones de ponerle el nombre que se merece.

## Seguramente será borrado

Más de una vez me pasó que terminé borrando el primer test que escribí, después de haber escrito varios y
ver _de qué va la cosa_. Es completamente normal; a medida que vamos aprendiendo, también vamos organizando
el conocimiento y clasificando lo que ocurre en nuestro sistema en casos de una mejor manera. Y podemos ir
hacia atrás y decir: "¡este test no tiene mucho sentido ahora!" _Delete_. Hemos aprendido y reflejado dicho
aprendizaje, eso es lo que cuenta.

## Redondeando

¿Vale equivocarse? **Hay que equivocarse**. Y aprender, y volver a equivocarse; muchas veces, cuantas más,
mejor. Que ese primer test sea el disparador de muchos más; y que puedas junto a tu equipo subirte al _tren
hacia la calidad_, al que lamentablemente pocos proyectos llegan.
