# Explícalo con tus palabras

Cuando tengas el comprobador en verde, contesta aquí sin mirar los apuntes (2–4 líneas cada una).
Es lo que te preguntarán en la defensa de la práctica.

1. ¿Qué pasa si a Nginx le falta el bloque `location ~ \.php$`? ¿Por qué es peligroso?
Si falta el bloque PHP en Nginx, los ficheros .php se descargarían como si fueran texto plano o mostrarían código fuente en vez de ejecutarse. Es muy peligroso porque cualquiera podría ver contraseñas, las  claves de bases de datos, etc; secreta del servidor.

2. ¿Para qué sirve `try_files $uri =404;` dentro del bloque de PHP? ¿Qué hace Nginx con un `.php` que no existe, con él y sin él?
try_files $uri =404; comprueba que el archivo PHP exista antes de enviarlo a PHP-FPM.Con él, si el archivo no existe, Nginx corta y da un error 404; sin él, Nginx le pasaría peticiones de archivos inexistentes a PHP-FPM, lo que puede causar fallos de seguridad o errores innecesarios.

3. En `pedido.php`, ¿por qué el XSS se arregla al **imprimir** (con `e()`) y no al leer el formulario?
El XSS se arregla al imprimir porque el peligro no es que el usuario escriba código malicioso (como <script>), sino que la página web lo interprete como HTML o JavaScript al mostrarlo. La función e() transforma esos símbolos en texto seguro justo antes de mostralos en pantalla.

4. ¿Por qué en producción `display_errors` va a `Off`, pero `log_errors` a `On`? ¿Qué aprendería un atacante de un error en pantalla?
display_errors = Off oculta los errores a los usuarios para que no vean rutas internas, versiones de software o datos sensibles que un atacante usaría para hackear la web. log_errors = On los guarda de manera privada para que los administradores puedan revisarlos y solucionar fallos.

5. Explica de dónde sale tu `pm.max_children`. ¿Qué pasa si te quedas corto? ¿Y si te pasas?
El pm.max_children sale de calcular cuánta memoria libre hay restándole al total lo que consume el sistema, y dividiendo el resultado entre lo que gasta un worker. Si te quedas corto, la web se congelará o dará errores 504 cuando haya muchos usuarios; si te pasas, el servidor se quedará sin memoria RAM y se colgará por completo.
