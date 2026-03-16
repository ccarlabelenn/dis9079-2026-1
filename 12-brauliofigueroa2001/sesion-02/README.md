# sesion-02

lunes 16 marzo 2026

## primera parte

- hola no tengo grupo aún

- si escribo comilla invertida y bash, dentro de lo que esté en bash quedará en un color/texto específico, bash es un lenguaje distinto. Esto es como cuando ponía comillas cpp, código y cierre comillas

- README.md, es muy importante, es donde están las instrucciones de cómo usar un software o un hardware. La documentación técnica la mostraremos siempre en markdown

- ejemplo, la documentación de mateo <https://github.com/matbutom> es muy buena
  
- <https://pandoc.org/>

- instalaremos arduino, probablemente es de los últimos cursos en los que Aarón enseñará arduino

- <https://oshwa.org/>

- las herramientas que utilizaremos serán opensource

- instalar biblioteca de arduino mqtt, arduino r4 (ya la tenía) y arduinographics. También actualicé arduino a 2.3.8

- cuando cambian el primer numero de 1.2.3 cambian muchas cosas y se pueden romper también, si cambian el segundo 1.3.3 cambian cosas de adentro pero lo principal se mantiene

- instalaremos visualstudiocode pero más adelante

- homebrew nos ayuda a instalar cosas de manera más fácil, ejemplo si quiero descargar mosquitto, introduzco el nombre teniendo ya homebrew y lo busca y lo instala todo solo, soluciona todo

- ejemplo en clases , el setup inicia con serial.begin(9600), lo único que pasa acá es prender el puerto serial ```

``` cpp
// ejemplo00
// imprime la sigla del curso en el monitor serial
// luego espera 1 segundo e imprime :)
// funciona con Arduino Uno R4 WiFi
// marzo 2026
// por montoyamoraga para disenoUDP

// crear instancia de ArduinoLedMatrix
// con nombre pantalla
// esto lo comenté porque interfería ArduinoLEDMatrix pantalla;

void setup() {
  // iniciar puerto serial
  Serial.begin(9600);
}

void loop() {
  // imprimir en consola
  Serial.println("dis9079");
  // esperar un segundo
  delay(1000);

  // imprimir en consola
  Serial.println(":)");
  // esperar un segundo
  delay(1000);
}
```

- este código nos muestra una frase en el monitor serial y una carita feliz cada cierto tiempo

``` cpp
// ejemplo01
// imprime la sigla del curso en la pantalla led
// de la Arduino Uno R4 WiFi
// basado en
// https://docs.arduino.cc/tutorials/uno-r4-wifi/led-matrix/#scrolling-text-example
// marzo 2026
// por montoyamoraga para disenoUDP

// incluir bibliotecas
#include <ArduinoGraphics.h>
#include "Arduino_LED_Matrix.h"

// declarar instancia de ArduinoLEDMatrix
// con nombre pantalla
ArduinoLEDMatrix pantalla;

void setup() {

  // iniciar puerto serial
  Serial.begin(115200);

  // inicializar pantalla
  pantalla.begin();

}

void loop() {

  // definir nuevo dibujo
  pantalla.beginDraw();

  // definir trazo
  pantalla.stroke(0xFFFFFFFF);

  // definir velocidad de deslizamiento
  pantalla.textScrollSpeed(100);

  // definir texto
  const char texto[] = "    diseno udp dis9704 interacciones inalambricas    ";
  
  // definir tipo
  pantalla.textFont(Font_5x7);

  // definir inicio del texto
  pantalla.beginText(0, 1, 0xFFFFFF);

  // imprimir el texto
  pantalla.println(texto);

  // deslizar hacia la izquierda
  pantalla.endText(SCROLL_LEFT);

  // fin del dibujo
  pantalla.endDraw();
}
```

- este segundo código nos muestra un menasje dentro de la pantallita que viene en el arduino en físico
- este código utiliza la velocidad 115200 en serial.begin, es otro tipo de velocidad distinto a 9600

### mosquitto

- qué es mosquitto? un broker mqqt, no sé que es eso no entendi mucho

- todos los arduinos se conoectarán al servidor mosquitto

- hashtag espacio es otra forma de hacer comentarios, es otro lenguaje si

- -h es ayuda (help)

- le debo decir al arduino que se conecte al wifi y al servidor mosquitto

- los mensajes se mandan a través de canales

- en el ejemplo, int port = 1883; es que toca la puerta para conectarse a ese puerto

- debe conectarse al wifi, luego  mosquitto,
