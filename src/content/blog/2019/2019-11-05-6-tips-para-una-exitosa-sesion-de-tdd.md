---
title: 6 tips para una exitosa sesión de TDD
author: Nahuel Garbezza
pubDatetime: 2019-11-05T12:00:00
postSlug: 6-tips-para-una-exitosa-sesion-de-tdd
canonicalUrl: https://blog.10pines.com/es/2019/11/05/6-tips-para-una-exitosa-sesion-de-tdd/
featured: true
draft: false
tags:
  - TDD
description: "TDD es ~95% práctica. La teoría seguramente la conozcas o la hayas escuchado, el ciclo de Red-Green-Refactor te sea familiar y sepas qué es lo que se hace en cada paso. La práctica es lo difícil, y siempre es bueno tener una cierta guía a medida que vas practicando. Esta es una simple lista de cosas que funcionaron para mí, y que quizás funcionen para vos."
---

1. **No se permite análisis parálisis:** Es inevitable a veces pensar demasiado, incluso lo vemos como algo bueno... pero también consume tiempo, y el tiempo es mejor invertirlo construyendo software, en lugar de pensar cómo debería construirse. Preguntas como "¿cómo puedo llamar a este test?", "¿es mejor crear un objeto separado para esta lógica o no?" aparecen todo el tiempo. Esa clase de preguntas son para hacérselas una vez que el test esté verde, no antes. Puede pasar que no hayas construido lo suficiente como para tener respuesta a esas preguntas, así que si no las podés responder, TDD te va a guiar tarde o temprano a encontrarlas.
2. **No olvidar el paso 3:** A veces logramos que un test pase e inmediatamente escribimos uno nuevo. Pero el paso 3, el de refactor, es muy importante porque sirve para solucionar deuda técnica que quizás acabas de introducir. Va a ser más difícil de arreglar después. No olvides verificar tener buenos nombres, ¡ni tampoco olvides refactorizar los tests!. No hagas daño a tu yo del futuro. La deuda técnica genera interés siempre que no se pague, y es bueno ser conscientes respecto a medir ese interés.
3. **Plantear las aserciones primero:** un buen test sigue el patrón Act-Arrange-Assert (o Given-When-Then), pero eso no necesariamente indica que hay que escribir el test en ese orden. Personalmente prefiero escribirlos desde el final hasta el principio. Esto a ayuda a enfocarnos en obtener lo que esperamos, y luego escribir todo lo que sea necesario para que eso ocurra. Siguiendo este enfoque, probablemente al final vas a tener el contexto mínimo necesario para que el test tenga sentido. Es importante mantener la simplicidad, nadie quiere mantener tests con cientos de líneas de código de set up.
4. **Frenar la ansiedad:** A veces tenemos una idea sobre cómo será nuestra solución final, y no podemos resistirlo y rompemos con el ciclo de TDD (confieso, ¡me pasa todos los días!). Perdemos el estado de flow. Podríamos estar introduciendo un bug (solemos pensar que nuestras soluciones son "irrompibles"). Es un error pensar que por hacer TDD vamos a ir más lento. Hay que considerar los beneficios a largo plazo. También empezar un nuevo ciclo después de haber interrumpido otro tiene su costo, así que es conveniente ir a una velocidad constante, sin tomar atajos. Después de hacer varios ciclos probablemente notes que estás avanzando más rápido.
5. **Dejar casos borde para el final:** En general, los casos borde son los más difíciles de testear, comparado con los caminos "felices" (happy paths); y en términos de priorización, pueden quedar para el final. ¿Qué es la primer cosa que los futuros usuarios de nuestro sistema van a intentar hacer? Eso debería ser nuestra prioridad. A menos que ya sepas que es lo que no se espera que pase, y no lleva mucho esfuerzo testearlo.
6. **Tomar otro camino si estás estancado:** Supongamos que estamos estancados tratando de que un test pase, o incluso más atrás, escribiendo un test para que falle por primera vez. En general lo que recomiendo es que si luego de 10 o 15 minutos no podemos salir de ese estado, debemos ir por otro camino. Tanto diverger (dejar el test actual sin terminar e intentar otro) or volver hacia atrás borrando todo el trabajo hecho en el ciclo actual, son opciones válidas. En TDD estamos siempre siguiendo un camino, y a veces es sabio cuestionarse si el camino es el correcto, ir un paso hacia atrás y buscar una mejor dirección.

En resumen:

- No dejes que nada te bloquee.
- No dejes pasar oportunidades de mejora.
- Empezá por algo sencillo y lejos de ser perfecto, y avanzá de a pasos pequeños.

¡Y te va a ir bien!
