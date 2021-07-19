---
layout: post
title:  "Contar colaboraciones: una métrica que importa mucho"
date:   2021-07-18 20:00:00 -0300
comments: true
excerpt: Contar líneas de código es una métrica cuestionable. Veamos cuáles son sus ventajas y desventajas, y cómo podríamos tener una métrica superadora contando envíos de mensajes.
---

## Medir en el mundo del software

Es bien sabida la frase "no puedes mejorar lo que no puedes medir". También es cierto que si el instrumento de medición no es preciso, las conclusiones que podamos sacar al respecto tampoco lo van a ser. También es cierto que podemos elegir métricas solo porque nos resulta fácil contarlas o porque nos resultan convenientes.

También hay métricas que no nos ayudan a mejorar, solo quizás a sentirnos en un falso lugar de bienestar. Las llamadas métricas de vanidad. Por ejemplo, cantidad de commits en un intervalo de tiempo. Dice algo de la calidad de lo que hacemos? Una persona es mejor que otra porque hace más commits?

En resumen; hay que tener cuidado con lo que medimos y sobre todo, cómo actuamos en base a eso.

En esta ocasión les invito a reflexionar sobre la métrica de **cantidad de líneas de código** (SLOC, source lines of code). En mi opinión, es una métrica que sirve para algunas cosas y para muchas otras deja que desear y debemos apuntar a buscar mejores métricas.

## Qué nos pasa con las líneas de código

Un sistema puede tener un millón de líneas de código y ser un software muy saludable, desacoplado, con muchos tests. Mientras que otro pueden ser apenas unas cientas y ser el código más terrible que hayamos visto. Con lo cual, no la podemos usar para comparar un programa con otro.

El formato del código nos altera esta métrica. Por ejemplo podríamos tener un formatter en Java que a aquellos parámetros que tengan más de un largo x los ubique en una línea aparte (digo Java porque las firmas de métodos suelen ser más largas que en otros lenguajes). Ese extra de líneas hace que mi software sea peor? No tiene mucho sentido este razonamiento.

Siguiendo en la línea del formato, existe en algunxs programadorxs una afición por los one-liners, expresiones que resuelven algo largo o difícil con código que cabe en una línea. Esas solo suman 1 en nuestra métrica, pero cuanto suman en complejidad?

Y si lo pensamos muy bien, el hecho que escribimos diferentes líneas de código es una cuestión accidental, y el umbral de cuan ancha es una línea depende del ancho de nuestra pantalla o lo que se nos ocurra configurar en el *linter*. Incluso hay personas que al día de hoy se preocupan porque su código tenga líneas de no más de 80 caracteres, cuando este límite tenía sentido enla época de las tarjetas perforadas. Podríamos escribir todo nuestro código en una línea, poner un word-wrap o algún otro chiche visual para ver el código distinto y ahí si que jodemos bien jodida a la métrica.

Entonces, ¿debemos dejar de medir líneas de código? No creo que haya que tomar medidas tan drásticas, hay dos casos en los que le encuentro sentido a utilizarlas:

- **Ver evolución en el tiempo:** sabemos que no tiene sentido comparar diferentes módulos o programas en términos de líneas de código, pero un mismo módulo o programa contra otra versión anterior o posterior suya puede ser interesante. Podemos hacer un gráfico y ver la evolución durante semanas, meses o años. Y en base a lo que vemos, podemos percibir qué tanto crece o decrece nuestro software, y de qué manera lo hace. Algo esperado podría ser que crezca linealmente a medida que le agregamos funcionalidad. Si crece exponencialmente, estamos ante un potencial problema de mantenimiento. Si crece, aún sin agregar funcionalidad, puede ser un indicador de que estamos agregando parches a nuestro código. También puede bajar! Lo que puede indicar que estamos generando abstracciones o simplemente quitando funcionalidad con el objetivo de hacerlo más simple.
- **Encontrar *hotspots* en el código:** la idea de [hotspot](https://understandlegacycode.com/blog/focus-refactoring-with-hotspots-analysis/) (en castellano se podría traducir como "punto de atención") es interesante cuando analizamos sistemas heredados, que son muy grandes o simplemente que no conocemos. En sistemas así, es muy difícil hacer análisis precisos y detallados porque ni siquiera sabemos por dónde empezar a mirar. Entonces, hacer un pantallazo rápido y ver, por ejemplo, líneas de código por archivo o clase podría ser un indicador que al menos nos ayude a encarar este sistema no por "el" lugar correcto, sino por "un" lugar que necesita nuestra atención. Este análisis, combinado con algunos otros, como por ejemplo el de churn vs. complexity (cuantas veces lo cambiamos vs. qué tan complejo es) nos puede resumir un módulo o una parte de nuestro sistema a un número que podemos trabajar en reducir a lo largo del tiempo.

En ambos casos lo que podemos hacer es una aproximación que debemos complementar con más métricas y análisis, no nos sirven para analizar en detalle parámetros de calidad.

## Una métrica superadora

Si no medimos líneas de código, entonces, ¿qué podemos medir? Mi propuesta es bastante simple: **medir cantidad de colaboraciones**. Es decir, cada vez que se le envía un mensaje a un objeto.

Cuando, dentro de un método, enviamos un mensaje a un objeto, estamos coordinando la realización de una parte de nuestro problema. Si enviamos muchos mensajes, a nuestro propio objeto o a otros, lo que está pasando es que quizás estamos tomando mucha responsabilidad y nos estamos perdiendo la oportunidad de delegar (recordemos la frase "¿no podría hacerlo otro?" de Homero Simpson como slogan de campaña... 🤣).

También lo que ocurre es que nos genera una carga mental alta leer varias colaboraciones. En líneas generales, entender lo que hace un método nos debería llevar unos pocos segundos. Claramente, tener más colaboraciones aumenta ese tiempo de lectura, a veces de manera lineal y a veces hasta de manera exponencial.

## Cómo seguimos

En próximos posts, vamos a ver cómo calcular esta métrica en diferentes lenguajes de programación. Es un lindo (¡y útil!) ejercicio de metaprogramación (o sea, programación cuando te enfocás en el dominio de la programación). ¡Nos leemos pronto!
