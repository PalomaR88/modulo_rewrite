# Módulo rewrite
El módulo rewrite nos va a permitir acceder a una URL e internamente está accediendo a otra. Ayudando por los ficheros **.htaccess**, el módulo reqrite nos va a ayudar a formar URL amigables que son más consideradas por los motores de búsquedas y mejor recoradadas por los humanos. Por ejemplo estas URL:

~~~
www.dominio.com/articulos/muestra.php?id=23
www.dominio-com/pueblos/pueblo.php?nombre=torrelodones
~~~

Es mucho mejor escribirlas como:
~~~
www.dominio/articulos/23.php
www.dominio.com/pueblos/torrelodones.php
~~~

> Ejemplo:

Se levanta el módulo rewrite:
~~~
a2enmod rewrite
systemctl restart apache2
~~~

Se instala el paquete php para apache2:
~~~
sudo apt install libapache2-mod-php
~~~

### Reescriir URL
En un fichero .php se escribe las órdenes correspondientes.

En .htaccess:
~~~
Options FollowSymLinks
RewriteEngine On
RewriteRule ^Operacion.html$ operacion.php
~~~

### Cambiar la extensión de los ficheros
Para cambiar la extensión:
~~~
RewriteReule ^(.+).do$ $1.html [r,nc]
~~~

### Crear URL amigables
Otra forma de formar direcciones, con variables:
~~~
RewriteBase /
RewriteRule ^([a-z]+)/([0-9]+)/([0-9]+)$ operacion.php?op=$1&op1=$2&op2=$3
~~~

### Acortar URL
~~~
RewriteRule ^buscar busqueda/buscar.php
~~~


## Uso de RewriteCond
Permite especificar una condición que, si se cumple, se ejecuta la directiva **RewriteRule** posterior. Se pueden poner varias condiciones con RewriteCond, en este caso, cuando se cumplen todas, se ejecuta la directiva Rewrite posterior.

- **%{HTTP_USER_AGENT}**: información del cliente que accede. Por ejemplo, podemos mostrar una página distinta para cada navegador:
~~~
RewriteCond %{HTTP_USER_AGENT} ^Mozilla
RewriteRule ^/$ /index.max.html [L]
RewriteCond %{HTTP_USER_AGENT} ^Lynx
RewriteRule ^/$ /index.min.html [L]
RewirteRule ^/$ /index.html [L]


- **%{QUERY_STRING}**: Guarda la cadena de parámetros de una URL dinámica. Por ejmplo: Teníamos un fichero indx.php que recibía un parámetros lang, para traducir el mensaje de bienvenida:
~~~
http://localhost/index.php?lang=es
~~~

Se quiere seguir utilizando la misma forma de traducir:
~~~
RewriteCond %{QUERY_STRING} lang=(.*)
RewriteRule ^index.php$ /%1/$1
~~~


- **%{HTTP_REFERER}**: Guardar la URL que accede a nuestra página y %{REQUEST_UTI} guarda la URI, URL sin nombre del cominio. Se evita así el Hot_Linking, o uso de recursos de tu servidor desde otra web. Por ejemplo, un caso muy común es usar imágenes alojadas en tu servidor puestas en otra web:

~~~
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)?dominio\.com/ [NC]
RewriteCond %{REQUEST_URI} !hotlink\.(gif|png) [NC]
RewriteRule .*\.(gif|jpg|png)$ http://www.dominio.com/image/hotlink.png [NC]
~~~


