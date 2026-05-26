# sesion-04

lunes 30 marzo 2026
descargar mqtt
enviar y resibir mensajes con el arduino
proxima clase avanzar en la solemne

## mqtt

El protocolo MQTT (Message Queuing Telemetry Transport) es un sistema de comunicación muy ligero utilizado para enviar y recibir mensajes entre dispositivos conectados a internet. En proyectos con Arduino, MQTT permite que placas como Arduino o ESP32 intercambien información en tiempo real mediante un servidor llamado broker. Este protocolo es muy usado en proyectos de Internet de las Cosas (IoT), ya que consume pocos recursos y funciona bien incluso con conexiones lentas.

Para enviar mensajes con Arduino, el dispositivo utiliza la función publish, que manda información a un tema específico llamado topic. Por ejemplo, un sensor puede publicar datos de temperatura en el topic “casa/temperatura”. Para recibir mensajes, Arduino utiliza la función subscribe, que le permite escuchar ciertos topics y reaccionar cuando llega nueva información, como encender un LED o activar un motor. Todo esto normalmente se programa usando librerías como PubSubClient dentro del Arduino IDE.

MQTT es muy útil en automatización y proyectos interactivos porque permite conectar múltiples dispositivos entre sí de manera sencilla. Por ejemplo, un Arduino puede enviar datos a una aplicación móvil, mientras otro Arduino recibe instrucciones desde internet para controlar luces o sensores. Gracias a su rapidez y bajo consumo de datos, MQTT se ha convertido en una de las tecnologías más utilizadas en sistemas inteligentes, domótica y monitoreo remoto.