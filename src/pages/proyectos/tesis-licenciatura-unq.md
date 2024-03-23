---
layout: ../../layouts/TopLevelPage.astro
title: "Tesis de Licenciatura (UNQ)"
---

El trabajo de tesis que realicé para obtener la
[Licenciatura en Informática](https://www.unq.edu.ar/carrera/58-licenciatura-en-informatica/) se titula _"Mejorando el
ambiente de programación Cuis Smalltalk con refactorings esenciales"_, y su resumen es el siguiente:

> Los refactorings automatizados (transformaciones de un programa sin variar su comportamiento) son parte de la mayoría
> de los ambientes de programación y son necesarios para trabajar con eficiencia y eliminar la probabilidad de errores
> triviales, dejando las tareas más complejas para quien escribe el programa.
>
> En el caso de Smalltalk, el primer lenguaje en tener refactorings automatizados como parte de un ambiente de programación,
> existe una herramienta llamada Refactoring Browser, que fue implementada en varias distribuciones. Refactoring Browser
> posee dos importantes desventajas: tiene su propia representación del árbol de sintaxis abstracto (adicional a las que
> ya tenga cada distribución), y los refactorings no son capaces de preservar el formato del código existente.
>
> Cuis Smalltalk es una distribución de Smalltalk de código abierto, multiplataforma yorientada a la simplicidad. Cuis
> posee una nueva implementación de refactorings en la que se apunta a resolver los problemas históricos del Refactoring
> Browser, y así poder tener una mayor variedad de refactorings, con implementaciones robustas y que sean capaces de
> utilizarse de manera versátil para lograr refactorings más avanzados.
>
> En este trabajo se presenta el desarrollo de dos refactorings: Extract Method y Extract Variable, para Cuis Smalltalk.
> Dichos refactorings son al día de hoy parte de la distribución y utilizados frecuentemente por toda la comunidad, tanto
> en la academia como en la industria. Para poder implementar los nuevos refactorings fueron necesarios cambios
> adicionales a Cuis (principalmente al parser) los cuales fueron cuidadosamente introducidos para contribuir positivamente
> a la calidad del código.

El video de la defensa lo pueden ver por aquí:

<div style="position: relative;overflow: hidden;width: 100%;padding-top: 56.25%;">
    <iframe style="position: absolute;top: 0;left: 0;bottom: 0;right: 0;width: 100%;height: 100%;" src="https://www.youtube.com/embed/rYRJ1X7b9J8" title="Tesis programación Garbezza" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

La presentación que utilicé durante la defensa:

<script async class="speakerdeck-embed" data-id="d0bec7ea327f40ed90e418eb80b365e0" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

Y el informe completo puede descargarse [desde este enlace](/public/tesis-nahuel-garbezza-unq.pdf).
