---
title: "Hugo Blog en Netlify"
date: 2018-03-22T18:31:35+01:00
draft: true
---

# Tu blog personal sin depender de nadie

Seguro que muchas veces te has planteado expresar tus ideas, conocimientos, aficiones o experiencias a través de un blog pero nunca te has atrevido porque no posees las habilidades informáticas necesarias y 
tu _cuñao_ el informático no quiere ayudarte.

También puede ser que no sepas que plataforma elegir para alojar tu blog empezar a escribir en Internet. Hay muchas, de diversas compañías y con distintas prestaciones. Ahora no vamos a entrar a comparar las 
distintas plataformas, ni siquiera vamos a compararlas. Vamos a hablar de la que elegimos para nuestra 
primera experiencia.

## netlify.com

[netlify.com](https://netlify.com) es una plataforma en la que podrás alojar tu blog personal, o incluso la página web esa que estás desarrollando para tu pequeña empresa o _pyme_ o, porqué no, para ti mismo.

En [netlify.com](https://netlify.com) tienen planes que van desde lo _free_ hasta planes que pueden 
ofrecernos una estabilidad profesional para nuestro proyecto. Puedes echar un vistazo a sus 
[planes](https://www.netlify.com/pricing/).

Ahora para nuestro experimento vas a tener que crear una cuenta en su plan _free_ y pasamos al siguiente punto.

## github.com

No conoces todavía [github.com](https://github.com), pues debes conocerlo ahora mismo. 
Es un sitio en donde podemos depositar nuestros repositorios de código y, aunque es principalmente un sitio para desarrolladores de software, podemos alojar nuestro blog que se conectará a _netlify_.

Vale, pues entonces ahora mismo te tienes que crear una cuenta en [github.com](https://github.com) que también es _free_, la única _pega_ por llamarle de alguna manera es que tu repositorio (recuerda que es donde vas a alojar el contenido de tu blog) es público pero, no era eso lo que querías al fin y al cabo.

Una vez que hayas creado la cuenta, crea un repositorio nuevo que será el que vamos a conectar con nuestra cuenta de [netlify.com](https://netlify.com).

## hugo

Es el tercer componente de nuestro blog. Qué es **hugo**. Bueno, es un generador de sitios estáticos en pocas palabras. Y qué significa esto, bueno pues ni más ni menos que vas a poder estar desarrollando tu blog en tu ordenador personal y el generará toda la estructrura necesaria de ficheros _html_, _css_, imágenes y demás que posteriormente vamos a desplegar en las cuentas creadas anteriormente.

### Instalación de hugo

~~~
sudo apt install hugo
~~~

### Creación de mi nuevo blog

~~~
hugo new site minuevoblog
cd minuevoblog
hugo server -D
~~~

