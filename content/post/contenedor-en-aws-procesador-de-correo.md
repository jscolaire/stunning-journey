---
title: "Contenedor en AWS procesador de correo"
date: 2018-03-22T19:00:59+01:00
draft: false
---

# Contenedor en AWS procesador de correo

El sitio https://www.segurosdetodo.es tiene dos formularios para interactuar con sus clientes, uno para que se llame al cliente y otro para atender una petición de correo.

Puesto que inicialmente el [sitio](https://www.segurosdetodo.es), y actualmente, es generado estáticamente y no utiliza lenguajes de procesamiento que puedan hacernos estas funciones, desde un principio se recurrió a una aplicación externa que hiciera estas funciones.

Primero se realizó en *OpenShift*, la plataforma de desarrollo que Red Hat puso a disposición de los que quieran utilizarla, donde se desplegó una aplicación [Sinatra](http://sinatrarb.com).

Con el _update_ de versión, la aplicación no funcionaba en *OpenShit*, lo que probablemente era culpa mía y no en sí de la plataforma, por lo que se tuvo que migrar y pasamos al _mundo_ de [Amazon AWS](https://aws.amazon.com/es/) dónde se puede desplegar un contenedor de forma _free_ si no va a sobrepasar unos límites predefinidos, como es el caso que nos ocupa.

La cuenta en Amazon AWS en sí no es gratis, pero si tienes una cuenta _Prime_ en [Amazon](https://www.amazon.es/) ya la tienes en AWS.

## Actualización del contenedor

~~~
docker run --rm --name cwmail --p 3000:3000 -ti cwmail
~~~

Modificamos `app.rb` que es donde está toda la lógica de aplicación, que es bien poquita. Una vez modificados los cambios copiaremos el fichero al contenedor:

~~~
docker cp app.rb cwmail:/app/app.rb
~~~

Por último volveremos a hacer un _commit_ a la imagen:

~~~
docker commit -a @jscolaire -m "Actualización de cwmail" cwmail cwmail
~~~

Ahora tenemos nuevamente nuestra imagen preparada, pero ahora está con nuestros cambios.

## Subida a AWS

Ahora hay que subir esta imagen, con los últimos cambios, al contenedor que está desplegado en AWS. Para ello y una vez _logados_ en nuestra cuenta nos dirigimos [al repositorio](https://us-east-2.console.aws.amazon.com/ecs/home?region=us-east-2#/repositories) donde está alojado nuestro contenedor. En este caso sólo tenemos uno por lo que una vez seleccionado podremos ver el botón que nos enseñará los comandos para hacer un nuevo _push_ de la imagen. En este caso concreto serán los siguientes:

~~~
aws ecr get-login --no-include-email --region us-east-2
~~~

Con este comando obtendremos un _token_ que nos permitirá hacer _login_ y poder subir nuestro contenedor modificado. Obtendremos algo así:

~~~
docker login -u AWS -p eyJwYXlsb2FkIjoiakQ3SEh5azJlR0NZWnk2ZjVFVzlhUkJQeFV4aWg1YUZFODFw
d29DUzRTb3AyV3JoUHdZc3h3Q1VOV2pCeUJpNDBISmxkWWRvSS9hRngyR2JMSUVDRHY1NVN6TDJuUU1MWmFVZUhteUdsZVRyL1N1UDVTOXJ5ODI0ZU4zb1NGMEtzNUtOazNtTU9XZE1oR2VBd0Qrb0cyQllUNUF4aUhEdnYwVGh3dWszbGNaczFSdXVWR1o2TEQ2QUVaOXRzdmg1Ym10UXlQK2VmdlJnZUhkWkNHSjBuZldOMHVPb0hXRXdzNXJYZ1l1a2h0eDJzUGxDWFpWUm9HcEFleG9KRHNveUNNN1JVd2dNblQzUnhPTzVNZEZ4ZEN0MzlxUnYvTmE5R2xCU0s0aWpMc3dLVHgyR1FaWmJ3aVV6VXhneUFGc3lmV1hpMXVDOGFUMnY0TFBKeWNiWXhGdFhsaDlYRjV2LzNmQWJ6clVkcVltSHpTcHUxbkROa0YvbEc0cU0xN1psUmZ0VEsvb3lUNmgzK1lTbndlWThKa3d2QjA4VzhSL01zc0cwcklFYy8rckFtL1BCczV3QWtmTE92SGExMWRFMDMzTE5NcEgzdXZYb2tGM2xrdmVvYU1tTEJ5cytwQmp6bUw4K2lBQUEyTEUrU3F2T1dxcHFLUkhmVmphb3dsdmlPb25xZ1l1bXRvUUluNHMya29IcWZjb0tOaVdqeDlUZi9RNDdsNSt2azBXVTVqNk5sdzJFb01tK1pyZ2w1UzdWMEVwZkpUd3QzdDFCS3ZIZU9jSnZlQkNaaUI4RktkSmdkUUtoWVdjTnMwcEJlVWF4bCtpUTNuUWRlMWU1TTFycWhScjNlMGwzV1JwQXU4SGpyNEJMdzMvSUpid0JFSGZWbUt5RWI5UVNSRWpERUJlK0draVhKUmJnRmRVNTZhd1d0N3dBSVNFcDVuQU1LMENSczBDTi9KY3MxcG1NYm5tVGowbEN2NkJBYll6Nkx1L0NFUHl3NkRQT0JkMTR5REdzd2pZTFZpUzFJMDNGdktMYXlpSkpsTk9rc0hjNkZyVjVtamJhWmhnMEp2YjQxUzFvVXFSWk9EYStOWS9RWmVvNE9KdjlaL0c4b2FKTUlCMXA2MlZWSys2eDdlbHJ1dnVkVXhMQi9sNWYzWlVOMUdyVjFsbC83MUNpaURFaTc3MGg2bjhTN0V3UXkwaGRuajdaUkg4SDVHQ0plek9ZK2pjbmxlend1TWVZK0lJQm9Hd1BNUytoVC9yVXRlY1ZmZjl5SEg1MmJkVGcydlNSNDVBK2hreGlDT2JmUnVFWnVicG5zZVV1WERoTW02bjZNUDh5L0RjVHpvbGZqN1g3Z2FZZmthYi9EK2FRbTF1UzF6OWN5UmZ4dHBobWJFUkJpdWs2eCtkeHRFK0hVSlA5SlZjTC9sVmtVOTMvMTdreFNZTloxVFMzZ01WTlNCL0ppSjVlUy9VPSIsImRhdGFrZXkiOiJBUUVCQUhqQjcvaWd3TWc0TlB3YXVyeFNJWXg0SGZueHVHYy80OGJEd3Z3RHBOWVdaZ0FBQUg0d2ZBWUpLb1pJaHZjTkFRY0dvRzh3YlFJQkFEQm9CZ2txaGtpRzl3MEJCd0V3SGdZSllJWklBV1VEQkFFdU1CRUVERFV2b0xSZmhDc2hmekhwbndJQkVJQTdOdDhTdEJKOUdzKzVyaWZLVDJIOTIva0hDa29Oczg3MEcwVTFKWFhqcHpKLzlnQWNldUdwNGxjYkJ5d0dtNkZTNlNUd3ArdVdQVW5kTFpVPSIsInZlcnNpb24iOiIyIiwidHlwZSI6IkRBVEFfS0VZIiwiZXhwaXJhdGlvbiI6MTUyMTc4Nzg5MH0= https://125989207195.dkr.ecr.us-east-2.amazonaws.com
~~~

Copiaremos y pegaremos en nuestra _shell_ y a continuación vamos a ejecutar los siguientes dos comandos que desplegarán el nuevo contenedor en AWS.

~~~
docker tag cwmail:latest 125989207195.dkr.ecr.us-east-2.amazonaws.com/cwmail:latest
docker push 125989207195.dkr.ecr.us-east-2.amazonaws.com/cwmail:latest
~~~

Hay que tener en cuenta que esto funciona porque en realidad tenemos instalado el cliente de AWS en nuestro sistema.
