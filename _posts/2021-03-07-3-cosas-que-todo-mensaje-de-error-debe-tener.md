---
layout: post
title:  "3 cosas que todo mensaje de error debe tener"
date:   2021-03-07 17:00:00 -0300
comments: true
excerpt: "¿Cuál es el problema? ¿dónde está el problema? ¿cómo resolvemos ese problema? 3 preguntas que te ayudarán a construir cualquier mensaje de error"
---

## Introducción

Una de las cosas que como desarrolladorxs tendemos a subebstimar o a no prestar mucha atención es la escritura de mensajes
de error.

Voy a mostrar un ejemplo a lo largo del artículo, que recientemente tuve que implementar para [Testy](/proyectos#Testy).
Este proyecto tiene textos internacionalizados, y lo que quería lograr era validar que se estaba configurando un idioma
soportado por la herramienta. Si esto no ocurre, deberíamos mostrar un mensaje explicando qué sucedió.

## Cuál es el problema

Para empezar, el error debe poner en tema a quien lee. ¿Cuál es la naturaliza de este problema? ¿Es un valor inválido?
¿Es algo temporal o inesperado?. Lo más claro que pueda ser el mensaje, mejor. Pero, si estamos haciendo visible un mensaje
de error, quizás no querramos decir algo como `MySQL error 1022`, que es demasiado técnico; hay que interpretarlo y
convertirlo en un mensaje más amigable.

En nuestro ejemplo, el problema es que el idioma que estamos tratando de usar no está soportado por la herramienta
todavía. Pero, decir `"Idioma no soportado"` no es suficiente...

## Dónde estuvo el problema

Ahora que sabemos que ocurrió un problema, y quizás ya sepamos en qué parte del software se generó la falla. Pero, ¿cómo
llegamos a esa situación? ¿Hicimos algo malo o la herramienta falló? ¿Dimos algún valor inválido?

Volviendo al ejemplo, quizás querramos indicar cuál fue exactamente el dato que generó el problema, y hacerlo parte del
mensaje de error. Mejorando el texto anterior diríamos algo como `"Idioma 'klingon' no soportado"`. ¿Estamos bien? Casi,
podemos hacerlo aún mejor...

## Cómo resolvemos ese problema

Ahora ya tenemos más información, sabemos exactamente qué dato fue el que generó el problema. Pero entonces, ¿qué dato
debo usar? ¿Hay una lista de valores permitidos? Si era un problema temporal, ¿cuándo se recomienda intentar de nuevo?
Recordemos, un buen software es el que nos enseña cómo usarlo, mientras lo usamos.

Mejoremos nuestro ejemplo agregándole los valores posibles para esta configuración de idioma: `"Idioma 'klingon' no soportado. Por favor, elegir alguno de los siguientes idiomas: 'spanish', 'english'."`

## Conclusiones

No perdamos de vista estos 3 aspectos, y ante cada situación de error nos podemos hacer estas 3 preguntas y ver si
nuestro mensaje las está respondiendo. SI obtuviste un mensaje de error y tuviste que buscar en internet qué significaba,
es una señal de que es un mensaje de error pobre. No vale la pena "ahorrar tiempo" escribiendo mensajes de error breves.
Si te da curiosidad cómo implementé este ejemplo, aquí está [el código](https://github.com/ngarbezza/testy/blob/develop/lib/i18n.js#L106),
y aquí [el test](https://github.com/ngarbezza/testy/blob/develop/tests/core/i18n_test.js#L69) (claro, ¡tampoco olvides testearlo!)
