---
layout: post
title: "Lidiando con emergencias: algunas lecciones aprendidas"
date: 2021-11-12 07:00:00 -0300
comments: true
tags: 
excerpt: Qué hacer, y sobre todo, qué no hacer cuando un sistema se cae. Algunas ideas para atravesar lo mejor posible esta experiencia y aprender mucho en el proceso.
---

Cuando un sistema *se cae*, todo alrededor se cae... O bueno, quizás no para tanto pero a veces sí. Como en la canción.

Lidiar con una emergencia no es algo que se suele aprender en la universidad, o en *bootcamps* (pero qué bueno sería,
¿no?) con lo cual las experiencias reales son las que nos van enseñando.

En estos últimos años he formado parte de emergencias relacionadas a sistemas de todos los colores y sabores, algunas
veces en contextos muy desfavorables. ¿Que aprendí? ¡Bastante! Aquí algunos puntos interesantes a tener en cuenta...

### Mantener la calma

Es inevitable que en una situación así domine el nerviosismo, porque dependiendo de qué sistema se trate, las
consecuencias de estar caído pueden ser muy graves. Lo ideal es mantener en lo posible la cabeza fría y pensar con
claridad. Es más fácil decirlo que hacerlo: las primeras veces que me tocó estar en esta situación, además de sufrir
el estrés que genera de por sí, también experimenté parálisis, y en el otro extremo demasiada precipitación; ambos
extremos son perjudiciales. Si tenemos la posibilidad de trabajar en el inconveniente con más de una persona, ambas
pueden darse ánimo o irse relevando. Si estamos en soledad, alejarse un rato del teclado, despejar la vista y volver
con más energías siempre sirve. Un tip que nunca está de más es revisar dos veces antes de ejecutar un comando que
puede ser riesgoso, o verbalizarlo con alguien más que esté trabajando en la solución, con el fin de validar lo que
estamos haciendo.

### Comprender la gravedad y el alcance

¿Cuántas personas están afectadas? ¿Qué tan afectadas están? ¿Cuánto tiempo podrían permanecer en esa situación? En
resumen, ¿qué tan grave es el problema? Todas preguntas válidas a hacernos al momento de zambullirnos en la resolución.
A veces, no tenemos respuestas a todas estas preguntas; por eso es necesario **trabajar activamente en la
observabilidad** de nuestro sistema: conocer datos de usuaries (por ejemplo dónde se ubican, qué dispositivos usan),
inspeccionar cuestiones de salud general (como tasa de error, o valores de CPU y memoria), y métricas más
particulares/de negocio (tasa de conversión, usuaries teniendo éxito haciendo X tarea, etc).

### Coordinar esfuerzos

Si más de una persona está trabajando para resolver el incidente, es importante mantenerse en sincronía. No pisarse
y a la vez aprovechar lo más posible el tiempo de cada persona. Dividir el incidente en partes para conquistarlo.

En las emergencias que mejor ví manejadas hay algo que siempre sucede: alguien del equipo asume un rol de líder con
la capacidad de ver el problema desde una perspectiva más amplia, mientras que otras personas pueden estar trabajando
de manera enfocada activamente en la resolución. Este liderazgo puede durar toda la emergencia o puede ser más
situacional y otras personas tomarlo (por ejemplo, si trabajamos con personas en otras zonas horarias y en algún
momento hay que hacer un traspaso). Lo que es clave es intentar ser explícito y asertivo en la comunicación.

Otra cosa que caracteriza a un buen manejo de una emergencia es, sobre todo en aquellas que duran varias horas o días,
hacer chequeos frecuentes con las personas involucradas. *Parar la pelota*, reagruparse y pensar un poco
estratégicamente.

### Mantener comunicaciones y registro de lo que va sucediendo

Es lógico que a medida que intentemos resolver el problema hagamos cambios (que pueden impactar positivamente o
negativamente). Por ejemplo, cambiar una variable de ambiente o reiniciar algún servicio.

Por ejemplo, si cambio una variable de entorno, recomiendo guardar el valor anterior, y restaurarlo si eso no resolvió
el problema. Si reiniciamos un servicio, y sabemos que va a tardar x minutos, podemos avisar a las personas que podrían
estar afectadas. También si hay más personas desarrollando puede ser necesario avisar que no se hagan nuevos despliegues
o cambios que podrían alterar aún más la situación. Recordemos que el objetivo es fijar la mayor cantidad de variables
posible.

En mi experiencia, esto funciona bastante bien cuando se forma un equipo para resolver la emergencia, y **una persona
asume ese rol**. Entonces el resto está enfocada en la resolución del problema propiamente dicho, y la persona
designada sirve de nexo con el resto de la organización y/o usuaries afectades.

### No sacar conclusiones apresuradas

Uno de los consejos más importantes que me gustaría destacar. A veces, por desear que el problema esté resuelto,
podemos autoconvencernos de que una solución es la definitiva. Seguramente surjan hipótesis (ej: *"debe ser la versión
de la base de datos que tenemos en producción"*) Es importante validar estas hipótesis antes de intentar una solución.
También, entender el impacto que podría tener cuando hagamos un cambio. Recordemos que debemos intentar, en lo posible,
de no empeorar la situación alterando más cosas. Nuestro rol es como el de un cirujano: estamos operando con un alto
nivel de riesgo, pero debemos valernos de todas las señales que nos da el cuerpo (en nuestro caso el sistema) que
podamos monitorear, y debemos saber que muchas cosas que intentemos van a tener consecuencias.

### Decisión: fix, parche, workaround o rollback

A muy alto nivel, una emergencia podría atacarse con alguno de estos 4 enfoques:

- *Fix*: intentar arreglar el problema de la manera correcta y definitiva. Por ejemplo, si el problema es un bug en el
código, escribir y poner en producción el código que lo arregla, idealmente con sus correspondientes pruebas
automatizadas que nos permitan detectar preventivamente si ocurre de nuevo.
- *Parche*: intentar una solución temporal, que potencialmente nos pueda sacar más rápido de la emergencia. Esto nos
deja deuda técnica que deberemos resolver una vez que estemos de nuevo en un estado de normalidad.
- *Workaround*: No resolver el problema, sino a recurrir a una solución con la que podamos vivir mientras el problema
se está solucionando. Por ejemplo: si un proceso automatizado está caido, ¿podemos seguir haciéndolo pero de manera
manual?
- *Rollback*: restaurar el sistema a su estado anterior, para poder trabajar con más tranquilidad en el problema. Hay
casos en donde se puede hacer esto (por ejemplo, cuando sospechamos que un reciente cambio causó el problema), otros
en donde no (por ejemplo, si estamos ante un ataque de DoS)

No es necesario elegir uno, si hay varias personas involucradas se pueden explorar varias soluciones en paralelo;
generalmente vamos a elegir la que más rápido nos saque de la emergencia.

### Recolectar aprendizajes

Una vez resuelta la emergencia, y de vuelta en el ritmo normal de trabajo, inmediatamente es interesante hacer una
reflexión para comprender por qué pasó lo que pasó, cómo podríamos haberlo detectado antes que se vuelva un problema,
y que podríamos hacer mejor en el futuro. Para esto recomiendo las sesiones de *post mortem*, que es una forma de
estructurar este análisis de manera que, por un lado, deje registro de lo sucedido (la gravedad, quienes estuvieron
involucrades, los tiempos e hitos desde que se detecto el problema hasta que se solucionó por completo) y también
sirva como espacio para **reflexionar sin buscar culpables**, y preguntarse *por qué* la cantidad de veces necesaria.

## Resumiendo

- En lo posible, definir un equipo de trabajo con roles claros y explícitos
- Mantener la comunicación con el resto de personas involucradas en el problema (¡esto incluye a usuaries también!)
- Mantener la calma y no apresurarse con soluciones
- Tener un pensamiento "fuera de la caja" y pensar posibles soluciones a corto plazo
- Intentar no agravar la situación introduciendo más cambios
- Luego de resuelto el problema, analizar en detalle qué sucedió y pensar oportunidades de mejora
