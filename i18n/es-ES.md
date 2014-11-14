Guía de Estilo-Scss
===================

Manera de Pensar
----------------
* [OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/) / [SMACSS](http://smacss.com/)
* Mejora Progresiva
* Desarrollo Responsivo: Móviles Primero

Tabla de Contenidos
-------------------

1. [Estructura de Directorio](#folderstructure)
2. [Estructurar un Módulo](#structure-a-module)
3. [Importar Archivos](#importing-files)
4. [Convenciones de Nombrado (BEM/SMACSS)](#naming-conventions)
5. [Comentarios (Sassdoc)](#comments)
6. [Variables](#variables)
7. [Manejando Puntos de Ruptura: Consultas de Elementos](#handling-breakpoints)
8. [Categorizando Reglas-CSS](#categorizing-css)
9. [Cosas-CSS](#css-stuff)
10. [Otros](#other)

<a name="folderstructure">Estructura de Directorio</a>
-----------

Tener una buena estructura para tu proceso de desarrollo es muy importante y la base de todo. Deberías prestar atención a que la base es casi la misma en cada proyecto – te ayudará a ti y a tus compañeros de trabajo a incorporarse más fácilmente en nuevos proyectos. Te mostraré mi estructura personal (basada en SMACSS):

```sass
scss/
|- _base/
|  |- _config.scss
|  |- _presets.scss
|  |- _headings.scss
|  |- ...

|- _layouts/
|  |- _l-base.scss
|  |- _l-grid.scss

|- _modules/
|  |- _m-buttons.scss
|  |- _m-tabs.scss

|- application.scss

stylesheets/
|- application.css
```

<a name="structure-a-module">Estructurar un Módulo</a>
-----------

¿Una estructura para módulos? ¿No es demasiado? No, no lo creo. Un módulo puede tener tantos tipos diferentes o volverse bastante complejo, que una estructura bien planeada te ayudará mucho.


```sass
@charset "UTF-8";

// Config
$button-bgcolor: $blue;
$button-fontcolor: $white;

// Base
.m-button {}
.m-button--primary {}
.m-button--secondary {}
```

### El módulo tiene ~~tres~~ dos secciones diferentes
* Primero tenemos un área para Config. Ahí defino colores específicos u otras cosas.
* Ahora la parte principal del archivo, Base. Hay que escribir aquí todos los estilos para todos los tipos del módulo.
* ~~El pie de página llamado States. Aquí importo los estados para el módulo actual. No más.~~

<a name="importing-files">Importando archivos</a>
-----------

En un proyecto hay muchas parciales. No me gusta incluir archivos en otros archivos. Prefiero tener un solo archivo como `application.scss` y ahí incluir todos los demás. Para esto he creado una estructura como en éste ejemplo:

```sass
// Base
@import "_base/_config",
        "_base/_presets",
        "_base/_headings";

// Layouts
@import "_layouts/_l-default";

// Modules
@import "_modules/_m-buttons",
        "_modules/_m-tabs";
```

### Resumen:
* Estructura en `base`, `layouts` y `modules`
* Para cada sección declara `@import` sólo una vez
* Retira el final del archivo

### No hagas explotar tus archivos
Si tienes un proyecto grande con muchos módulos y variantes, entonces llegas al punto de pensar acerca de la estructura actual. Detonas tu archivo con un montón de código y es difícil mantenerlo.

Debido a esto necesitamos una pequeña mejora de la estructura y agregar algunas carpetas, si notas que tu módulo necesita muchas líneas de código. Después crea más parciales pequeñas e inclúyelas en un archivo principal para éste módulo.

```sass
// Base
@import "_base/_config",
        "_base/_presets",
        "_base/_headings";

// Layouts
@import "_layouts/_l-default";

// Module
@import "_modules/_forms/_m-forms";
```

En el `_m-forms.scss` debes importar todas las parciales de forma, que están en tu carpeta `_forms` La estructura puede verse como esto en uno de tus archivos:

```sass
// Parciales
@import "_m-form-base",
        "_m-form-two-one-small",
        "_m-form-row",
        "_m-form-with-prefix";
```

**Debes decidir cuál prefieres. No puedo decir cual deberías usar, porque mi decisión está basada en el proyecto actual.**

<a name="comments">Comentarios (Sassdoc)</a>
-----------

Documentar el código siempre es muy importante. Si echas un vistazo al ambiente después de un tiempo, te preguntas que fue lo que escribiste. Es difícil comprender sin comentarios y ahí te doy un consejo, especialmente para Sass: usa *[Sassdoc](https://github.com/SassDoc/sassdoc)* Es una increíble herramienta para crear una sólida documentación para tus variables, mixins, funciones, etc. Sólo debes escribir muchos más comentarios, pero obtendrás mucho de ello.

```sass
/**
 * Obtener el ancho de una columna
 *
 * @author Tim Hartmann
 * @group addons
 *
 * @param {number} $col - define which column do you need
 * @param {number} $max-cols (12) - maximum columns
 *
 * @example scss
 * col(4);
 * // 25%
 */
@mixin col($col, $max-cols: 12) {
  width: (100%/$max-cols)*$col;
}
```
*En la primera línea de tu comentario, da una breve descripción acerca del snippet. Esto ayuda mucho a tener un resumen rápido.*

`@author`: Declara la persona que escribió este snippet <br>
`@group`: Agrupar snippets es una gran forma de darle más estructura <br>
`@param`: Parámetros posibles para este mixin / función y escribe el tipo de cada uno <br>
`@example`: Escribe un ejemplo rápido de cómo puedes usarlo

**[Ejemplo](http://scss-components.com/sassdoc/)** <br/>
**[Documentación Sassdoc](https://github.com/SassDoc/sassdoc)**


<a name="naming-conventions">Convenciones de nombrado (BEM/SMACSS)</a>
-----------

### Arquitectura Bloque, Elemento, Modificador / Escalable y modular para CSS
Me encantan ambas maneras. Sin embargo, SMACSS es algo que todo desarrollador front-end que quiera escribir front-ends escalables debe leer. Jonathan Snook es el autor y escribió todo acerca de su experiencia en un libro. Combinar estos dos métodos es la mejor manera para mí de manejar convenciones de nombrado.

```sass
// Layouts
// Prefijo "l-"
.l-default {}

// Módulos
// Prefijo "m-"
.m-accordion {}

// Elemento hijo de acordeón
// Separador: "__"
.m-accordion__trigger {}

// Modificador de acordeón
// Separador: "--"
.m-accordion--plain {}

// Estados
// Generalmente con prefijos como "is-"
.is-active {}
.is-hidden {}
```

<a name="variables">Variables</a>
-----------

Al principio, creamos una paleta para cosas usadas globalmente como colores en el archivo `_config.scss` Si tienes algunas variables específicas, entonces colócalas en el archivo para el módulo. La legibilidad es muy importante y debido a esto uso separadores cuando nombro variables: un **guion.**

```sass
// Colores (global)
$white: #fff;
$blue: #1675d6;
$red: #e8402a;

// Nombrado específico
$button-bgcolor: $blue;
$button-fontcolor: $white;
```


<a name="handling-breakpoints">Manejando Puntos de Ruptura: Consultas de Elementos</a>
-----------

¿"Consultas de Elementos"? ¿Qué es lo que escribes, Tim? Eh, las “Consultas de Elementos” con Sass son mucho mejores que las consultas de medios (media queries). ¿Sabes cómo es tener todos tus estilos responsivos específicos en un archivo propio? En mi opinión, es mierda. Si escribes módulos deberías declarar su comportamiento responsivo ahí. **No en otro archivo.**


```sass
// Scss
.m-navigation {
  &:after {
    @media screen and (min-width: 767px) {
      content: '';
      display: inline-block;
      width: 100%;
    }
  }
  li {
    display: block;

    @media screen and (min-width: 767px) {
      float: left;
    }
  }
}

// CSS Generado
@media screen and (min-width: 767px) {
  .m-navigation:after {
    content: '';
    display: inline-block;
    width: 100%;
  }
}

.m-navigation li {
  display: block;
}

@media screen and (min-width: 767px) {
  .m-navigation li {
    float: left;
  }
}
```

No es importante para el desempeño el tener sólo una declaración de un media query. En consecuencia, podemos usarlo como **"Consultas de Elementos".** Lee más acerca de esto en [una entrada de blog](http://helloanselm.com/2014/web-performance-one-or-thousand-media-queries/) por Anselm Hannemann, quién se hizo la misma pregunta.


<a name="categorizing-css">Categorizando Reglas-CSS</a>
-----------

Si escribo Scss soy realmente consistente y quiero establecer reglas para todo tipo de cosas. Así pues, tengo un par de reglas para categorizar mis estilos.

```sass
// Elemento-Base
.element {
  margin: 10px;
  padding: 10px;

  border: 1px solid red;
  background: blue;

  color: white;
  font-size: 16px;

  -webkit-transform: translate3d(0,0,0);
}
```

* Caja
  * `margin`, `padding`, `display`, ...
* Borde y Fondo
  * `border`, `border-radius`, `background-color`, ...
* Texto
  * `color`, `font-size`, `text-transform`, ...
* Otras cosas
  * `animations`, `transform`'s, ...


<a name="css-stuff">Cosas-CSS</a>
-----------

### Evita IDs
Los ID’s son muy difíciles. Son demasiado específicos y mayormente necesarios para ganchos JavaScript. La mejor manera es usar clases.

### Evita `!important
Si necesitas la declaración `!important` algo anda muy mal con tu arquitectura CSS. Necesitas esto sólo si realmente quieres sobrescribir estilos. Si tu estructura de código es limpia no necesitarás esto. Es como usar un mazo para romper una nuez.

### Evita selectores hijos
¿Por qué evito selectores hijos? ¿No es una buena característica CSS, lo es? No, no lo creo. Cuando empecé con código, la use bastante seguido. Sin embargo, mientras más aprendí acerca de (S)CSS, más me di cuenta de que hace a un módulo demasiado específico. Depende de la estructura de tu HTML y puede hacer bastante difícil cambiar su estructura después. **¡Usa muchas más clases!**

```sass
// Mal ejemplo
// Selector-hijo
.m-tabs > li {}

// Buen ejemplo
// Nombre de clase
.m-tabs .m-tabs__trigger {}

// Mejor ejemplo
// Menos anidado
.m-tabs__trigger {}
```


<a name="other">Otros</a>
-----------

* Sobre todo dos espacios en blanco en lugar de tabulaciones para sangría
* Un espacio en blanco entre elementos y corchetes
* Saltos de línea entre estilos y nuevos elementos
* Saltos de líneas después de cerrar corchetes
* Usa códigos hexadecimales en vez de RGB para colores
* Convierte códigos hexadecimales a RGB con Sass
* mal ejemplo:
  * color: ```rgba(255,255,255,.3);```
* buen ejemplo:
  * color: ```rgba(#000, .3);```

```sass
// Mismos estilos para más de un elemento
// Separación con un salto de línea
.element,
.second-element {
  color: red;
}

.m-tabs {
  background: rgba($black, .3);
}
```