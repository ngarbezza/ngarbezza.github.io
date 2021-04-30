---
layout: post
title:  "Las 12 propiedades deseables de los tests según Kent Beck"
date:   2021-04-29 21:00:00 -0300
comments: true
excerpt: Un análisis sobre el artículo "Test Desiderata" escrito por Kent Beck, en donde se listan 12 propiedades deseables de los tests.
---

## Test Desiderata: “¿lo qué?”

En octubre de 2019, Kent Beck publicó un breve y excelente artículo llamado “Test Desiderata” donde resume qué características, cualidades o propiedades deseamos que los tests tengan; lo que (sorpresivamente para Kent) no es algo que esté cubierto por sus libros y posts sobre TDD y que, según sus palabras, “dio por sentado”. El resultado es un post con un excelente nivel de síntesis y de importancia del tema, un post al que vuelvo repetidas veces porque, en mi opinión, es clave para cualquier persona que practique TDD.

Adicionalmente, también hay unas mini-sesiones de videos con ejemplos prácticos que ilustran cada una de estas propiedades. Todo el material está disponible en Youtube:

https://www.youtube.com/watch?v=5LOdKDqdWYU

## Las 12 propiedades

Veamos a qué se refiere Kent con cada una de estas 12 propiedades:

> Isolated — tests should return the same results regardless of the order in which they are run.

El hecho de que un test esté aislado del contexto en el que corre es fundamental. Citando a Hernán Wilkinson: “el test tiene que estar en control de todo”. Esto implica asegurarse de un inicio limpio, una ejecución libre de intrusiones externas y no asumir cosas que puedan ocurrir por fuera del test.

> Composable — if tests are isolated, then I can run 1 or 10 or 100 or 1,000,000 and get the same results.

Consecuencia del punto anterior: los tests deberían poder agruparse de cualquier manera y los resultados deberían ser determinísticos.

> Fast — tests should run quickly.

Uno que damos por sentado, ¡y que es fácil de perder! Para que TDD funcione, tiene que haber feedback inmediato y, justamente, la parte de “inmediato” se refiere principalmente al tiempo entre que escribimos el test y vemos su resultado. Si esperamos dos minutos para ver el resultado del test que escribimos, cabe preguntarse ¿cuál será el efecto después de 50, 100 ciclos de TDD? Aprendemos, pero no aprendemos rápido. Tomamos atajos. Dejamos de hacer TDD. Para poner algo de números en contexto, recordemos la definición de unit test de Michael Feathers: un test que tarda menos de 1/10 de segundo en ejecutarse.

> Inspiring — passing the tests should inspire confidence

Lo más importante de un test no es que pase o no, sino que tanto al pasar como al fallar nos brinde seguridad. Que no reporte falsos positivos, ni falsos negativos. Que pueda sentir que puedo deployar mi código un viernes por la tarde luego de ver que todos los tests pasan. Siempre es importante ver a cada test fallar. Si estamos desarrollando con TDD eso va a ocurrir al inicio de cada ciclo. En caso de trabajar con tests existentes, podemos usar mutation testing para forzar a que el test falle.

> Writable — tests should be cheap to write relative to the cost of the code being tested.

Escribir tests automatizados siempre requiere de un esfuerzo. Hay que trabajar activamente para minimizarlo (las consecuencias de no hacerlo afectan otros puntos mencionados aquí también, como la legibilidad; y también el extremo de dejar de escribir tests porque nos resulta costoso). Crear datos de prueba debería ser sencillo, lo mismo con las aserciones, o cualquier otra cosa necesaria de contexto (configurar inputs adicionales, por ejemplo).

> Readable — tests should be comprehensible for reader, invoking the motivation for writing this particular test.

Recordemos que leemos código, mucho más de lo que escribimos. En varias ocasiones, leemos tests que otras personas escribieron y otras personas leen los tests que escribimos. Es importante que tanto la descripción del test como el código hablen de negocio y no de implementación, que refieran claramente al escenario que están testeando y que den el menor lugar posible a la ambigüedad. Con respecto al código, en este aspecto es muy útil contar con nuestras propias aserciones “de negocio”, que nos pueden abstraer un conjunto de aserciones básicas y ayudar a la persona que lee a comprender más rápidamente qué es lo que se espera. Recordemos también que la aserción es la parte más importante del test! Si nos perdemos en su lectura, probablemente no terminemos de comprender el test.

> Structure-insensitive — tests should not change their result if the structure of the code changes.

> Behavioral — tests should be sensitive to changes in the behavior of the code under test. If the behavior changes, the test result should change.

Cambios de implementación, refactorings y rediseños no deberían generar cambios en los tests. Si eso ocurre, es que tenemos tests denominados “de caja blanca”, muy comunes cuando se utilizan test doubles. Por el contrario, los buenos tests tienen aserciones que hablan sobre el comportamiento del sistema. Entonces si éste cambia, las aserciones deben cambiar también.

> Automated — tests should run without human intervention.

Volviendo a la cuestión de “el test debe estar en control de todo”: esto quiere decir que si un test necesita un dato de entrada, una confirmación o algún paso que eligen/deciden les usuaries, éso debe estar simulado con valores concretos y debe ser una precondición para que el test sea válido. De esta manera, un test se puede correr en un ambiente donde no es posible intervenir manualmente, como un servidor de integración continua.

> Specific — if a test fails, the cause of the failure should be obvious.

Partamos de la base del valor que tiene el feedback, en su carácter de inmediato, pero también en su carácter de preciso. Si un test dice solamente “failed” y no podemos asociar ese error con la parte del código causante del problema rápidamente, entonces muy probablemente el test necesite escribirse mejor.

> Deterministic — if nothing changes, the test result shouldn’t change.

Como desarrolladorxs, estamos todo el tiempo viendo diferentes resultados de tests, tanto exitosos como fallidos. Y siempre, muchas veces rápida e inconscientemente, correlacionamos el resultado con el input del test y su comportamiento para sacar conclusiones. Si no existe el determinismo, perdemos la confianza en este análisis, lo que resulta en confusión y desarrollos más lentos, porque, recordemos, una de las claves de TDD es ir avanzando en cada paso con muy alta seguridad y confianza en que nuestro sistema está funcionando como esperamos (en oposición a no escribir tests, o escribirlos luego de escribir el código).

> Predictive — if the tests all pass, then the code under test should be suitable for production.

Esto habla de la paridad que debe existir entre el ambiente de test y el/los ambiente/s productivo/s. De poco sirve un test que no refleja lo que realmente ocurre en producción. Y esto no sólo se refiere al test coverage, sino también a contemplar todas las variables que puedan causar bugs más allá de código que ningún test ejercita. Un ejemplo común es olvidar sobre qué datos estamos operando. Supongamos que en producción tenemos una inconsistencia de datos y que eso afecta el comportamiento de lo que estamos desarrollando. Deberíamos tener un test que considere ese caso, o arreglar la inconsistencia.

## Conclusiones

Podemos observar que la pérdida de cualquiera de estas 12 propiedades genera un impacto negativo en la experiencia de TDD inmediatamente. ¡Así que es muy bueno tenerlas en mente!

Para reflexionar: los tests que escribimos, ¿cumplen con estas propiedades? ¿en qué grado?
