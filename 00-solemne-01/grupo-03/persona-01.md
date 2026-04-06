# persona-01

- antokiaraa

investigaciones individuales

## sobre adafruit i/o

Instalación y configuración:

Paso 1: en Arduino IDE busqué la biblioteca Adafruit IO Arduino y la instalé junto con sus dependencias

![instalar biblioteca](./imagenes/biblioteca_adafruit.png)

Paso 2: descargué el código subido en la carpeta de mi grupo "enviarGrupo03" y su .h

Paso 3: me creé una cuenta en <https://io.adafruit.com/> con mi correo UDP y desde ahí copié mi username y la active key

Paso 4: en el código de arduino modifiqué las líneas donde debe ir el username, la active key, mi WiFi y la contraseña, verificando que el arduino esté conectado mandé el código

Error 1: compiló pero en el serial monitor me daba un cáracter raro, lo busqué y encontré [en esta página](https://forum.arduino.cc/t/solucionado-ayuda-monitor-serial-con-caracteres-ilegibles/473044/5) que el número del serial begin debe coincidir con la cantidad de baudios (bits por segundo) en el serial monitor

![error por baudios](./imagenes/error1_baud.png)

Aquí lo edité: 

![baudios solucionado](./imagenes/solucionado_baud.png)

Teniendo eso solucionado lo volví a enviar el código

Error 2: acá me estanqué un rato porque el serial monitor solo me tiraba "............" pero ningún mensaje, hasta que luego de un rato probando me apareció este mensaje y efectivamente como dijo Aaron me daba el error por el firmware del arduino, también me costó actualizarlo porque daba error y se quedaba pegada la aplicación, pero lo terminé solucionando solamente intentando de nuevo actualizarlo

![error por firmware](./imagenes/error2.png)

![actualizando firmware](./imagenes/actualizarfirmware.png)

Error 3: luego de solucionar esto volví a intentarlo pero me seguía dando los puntitos "....." por lo que averigüé es que el arduino está tratando de entrar al WiFi pero no lo logra, por lo que utilicé el hotspot de mi celular porque el iPhone tiene una opción de "máxima compatibilidad" para que pueda conectarse ya que sin eso estaria en 5Ghz y el arduino opera en 2.4GHz

Con esto solucionado vuelvo a tirar el código al arduino y funciona!

![valores en serial monitor](./imagenes/serialmonitor1.png)

Empezaron a aparecer los números en la sección "Feeds" en la página de Adafruit IO y viendo cómo funciona el dashboard creé uno con un block de "streams" para que ahi se puedan ir viendo los valores que envía el arduino, como la página de Adafruit estaba en inglés me guié de [esta página](https://mkelectronica.com/aprende-a-utilizar-la-plataforma-adafruit-io-para-tus-dispositivos-iot-parte-1/) para ir entendiendo cómo funcionaban 

![valores en feeds](./imagenes/valorfeed.png)

Así se ve en el Dashboard que creé: 

![streams en dashboard](./imagenes/dashboardstreams.png)

## sobre artista, diseñadora o producto que usa electrónica o computación inalámbricas
