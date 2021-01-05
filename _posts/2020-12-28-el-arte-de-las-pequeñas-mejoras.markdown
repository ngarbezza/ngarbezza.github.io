---
layout: post
title:  "El arte de las pequeñas mejoras"
date:   2020-12-28 10:00:00 -0300
comments: true
canonical_url: https://blog.10pines.com/2020/12/28/el-arte-de-las-pequenas-mejoras/
excerpt: "Refactorizar es lindo. Pero la realidad nos presenta restricciones (tiempo, alcance, tecnológicas, políticas). Ahí es donde la creatividad toma importancia y debemos lograr un alto impacto en poco tiempo, mientras hacemos crecer nuestro software."
---

## Intro

Refactorizar es _lindo_. Si fuera por nosotres, pasaríamos el día entero escribiendo y reescribiendo una pieza de software para que sea más entendible, más robusta, mantenible, y <inserte su requerimiento no funcional favorito aquí>.

Pero la realidad nos presenta restricciones (tiempo, alcance, tecnológicas, políticas). Ahí es donde la creatividad toma importancia y debemos lograr un alto impacto en poco tiempo, mientras hacemos crecer nuestro software.

## Experimentación

Algo que hacemos cuando trabajamos con TDD es buscar activamente el feedback lo más rápido posible. Con los refactorings debemos hacer lo mismo, aunque por alguna razón tendemos a apuntar a un cierto grado de perfección y feedback loops más largos...

Una cosa muy útil es tener líneas de trabajo experimentales, de vida muy corta y que puedan ser validadas por un servidor de integración continua.

Por ejemplo: sospechamos que un parámetro de configuración está haciendo que los tests sean más lentos; entonces ponemos el cambio en un branch, lo pusheamos y esperamos a ver el resultado de CI. Podemos observar 3 resultados:

1. confirmamos nuestra hipótesis y el tiempo de los tests baja, pudiendo asociar nuestro cambio a la baja del tiempo (dicho en otras palabras, no fue casualidad)
1. confirmamos que la configuración no tiene relevancia o incluso aporta negativamente a resolver el problema
1. el resultado es inconcluso, depende de más variables y/o necesitamos más información.

En caso de (2) o (3) el experimento se descarta y eventualmente se podrán generar otras variaciones que tengan en cuenta los resultados de este experimento.

A modo de referencia, últimamente de cada 10 intentos de mejoras que pruebo, estaré descartando entre 7 y 8. Pero no es algo para ver como fracaso, sino como aprendizaje para plantear cada vez mejores experimentos.

Y si es difícil medir, hay que ir un pasito atrás y asegurar eso antes de encarar grandes proyectos de mejora. Tener métricas “técnicas” es clave para medir nuestros experimentos.

Todo esto se puede resumir en dos palabras: "permitirnos fallar".

## El valor del equipo y la recurrencia

Una mejora pequeña pasa a ser insignificante cuando ocurre una vez cada tanto, y pocas personas lo realizan. El libro [Atomic Habits](https://jamesclear.com/atomic-habits), de una lectura muy amena y libre de complicaciones, lo resume muy bien. Lo recomiendo fuertemente a quienes la tengan difícil formando hábitos saludables.

Si una persona puede hacer 3 mejoras por semana, 5 personas harán 15. Si a esto le sumamos ser más cuidadosos introduciendo deuda técnica nueva, ¡estamos mejorando a pasos agigantados!

El siguiente video es un gran resumen de lo que es el papel del liderazgo en iniciativas como estas:

<iframe width="840" height="475" src="https://www.youtube.com/embed/fW8amMCVAJQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Divide y conquistarás

A veces suena un poco contradictorio que por un lado aplicamos esta técnica todo el tiempo en el código, pero luego, cuando queremos hacer un refactoring, buscamos el "todo o nada" y terminamos con un _pull request_ de 200 archivos que nadie quiere revisar, que es muy probable que genere conflictos fácilmente, y que si sale mal es más difícil de revertir. Sin contar que a medida que la cantidad de cambios crece, nuestra atención como revisores decae, con lo cual es probable que algún error simple como un error de tipeo no se detecte.

En grandes proyectos, y sobre todo en aplicaciones 24/7, una tarea simple como un _rename_ puede ser difícil de llevar a cabo. Con lo cual hay que pensarlo en partes, por ejemplo:

* Agregar el nombre nuevo
* Comunicar el cambio al equipo y explicar cuál es la solución preferida
* Deprecar el nombre viejo
* (tantas veces como sea necesario) reemplazar los usos progresivamente
* Borrar el nombre viejo

Al refactorizar, conviene ser más la tortuga que la liebre...

## Umbrales de tiempo

Refactorizando se suele perder la noción del tiempo. Después de un poco de experimentación, llegué a dos duraciones que me marcan el día a día de trabajo:

* **15 minutos** para explorar qué tan viable es un refactoring. En ese intervalo apunto a comprender con qué me estoy enfrentando, decidir qué atacar y qué no.
* **1 día** para integrar un conjunto de cambios. Cualquiera sean los cambios que realice, debería tener algo revisable por mi equipo, que pueda integrar en el transcurso de un día.

Ambas son duraciones que intentan ser cortas, justamente para evitar perderse en el refactoring. No lo veo como una regla que si no se cumple hay algo mal, sino más bien una heurística que sirve a modo de guía. Sé que al alcanzar estas marcas de tiempo debo tener un entregable/conclusión de lo que estuve haciendo.

Seguramente esto sea diferente para cada persona, tanto en la duración como en lo que se espera lograr en cada intervalo. Ya el ejercicio de pensarlo e iterarlo es muy interesante.

## Conclusión

Espero este artículo te haya inspirado y ya estés pensando qué mejora vas a introducir la próxima vez que tengas tu proyecto en frente. Si tenés alguna duda, ¡escribime! Si tenés algún tip que sientas que es relevante, ¡escribilo también! Refactoricemos y recordemos que lo perfecto es enemigo de lo bueno.
