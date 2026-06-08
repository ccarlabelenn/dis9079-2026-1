# sesion-13

lunes 08 junio 2026

## Apuntes

Cambios en Github del profesor 

y en 00-examen

se muestran los requisitos de la prueba

Investigación individual y lo que hicimos la semana pasada quedó de referente, por ahora lo importante es escribir el pseudo codigo del proyecto: son comentarios o diagramas que digan "has esto tantass veces" o "Como encontrar un maximo de un número", "encontrar el maximo de un número"

son como los planos de la casa antes de construir la casa y tiene valor en si mismo armar el pseudo codigo, con lista de materiales y dibujos si es que eso ayuda
y colocar un código de prueba con comentarios en consola simulando

Esto es programar (Varios días de planificación)

Y ahora procederemos a avanzar en clase

### Pseudocódigo

|paso|proceso|
|----|-------|
| 1 | Empezamos armar el circuito del cd 4015 en la protoboard, para empezar hacer las pruebas en fisico |
| 2 | Se empezo a haver la v2 del circuito cd4040, con las mejoras, con las conexciones para el cd4015 |
| 3 | 1er intento, no resulto, nos olivamos de conectar las placas entre si | 
| 4 | 2do intento prendio, se prendio hasta el 4 led pero no oscila ni se r | 
| 5 | Sospechas se cambiaron los leds del paso 5 y paso 6 |
| 6 |  fallas cambiamos leds quemados malas conexiones de leds resitencias que no tocaban la plaquita |
| 7 | se cambio la resistencia del 555 de 1k a 15k y logramos hacer el circuito mas estable |
| 8 | Al cambiar las resistencia dio mas tiempo al chip mas margen temporal al tiempo, un clock mas lento hace el circuito mas estable |


IDEA

BAsarnos en proyecto de pixela

Lo que hace es que captur una foto y usa un procesamiento de pixeles y aplica una paleta que hace que se vuelva retro, y en si el raspberry edita la imagen, usamos una aplicacion web puede procesar la imagen y pasarla a pixeles, "la lanza a un drive", con la rasapberry pi podemos visualizarlo con una pantalla y eso se sube a una nube sonde en el arduino queremos crear una galeriá publica conde se generan las nuevas imagenes, y podriamos con botones pasar las las fotos ya registradas anteriormente pero irian cambiando

### Pseudocódigo del proyecto

INICIO

Tomar foto

Enviar foto a la Raspberry Pi

La Raspberry Pi convierte la foto en pixel art

Guardar imagen

Subir imagen a la galería

Mostrar imagen en la pantalla

SI se presiona el botón siguiente
    Mostrar siguiente imagen
FIN SI

SI se presiona el botón anterior
    Mostrar imagen anterior
FIN SI

SI llega una nueva foto
    Convertirla a pixel art
    Agregarla a la galería
FIN SI

REPETIR TODO EL TIEMPO

