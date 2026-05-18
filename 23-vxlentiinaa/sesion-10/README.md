# sesion-10

lunes 18 mayo 2026

solemne 2

---

## Investigación

- Sensores: Dispositivo diseñado para detectar cambios en el entorno
- Actuadores: Dispositivo que recibe una orden o señal

Lo que queremos realizar en la solemne 2 es que desde la Raspberry pi envíe datos mediante un botón on/off hacía el Arduino y que este encienda una luz o emita algún sonido.

Entonces, nuestro pseudocódigo sería:

|Raspberry Pi Pico 2 W|Adafruit IO|Arduino UNO R4 wifi|
|---|---|---|
|Botón|MQTT|Led|
|ON/OFF|Feed: estado|Verde/Rojo|

1. Presionas el botón en la Raspberry > alterna entre ON y OFF
2. La Raspberry publica "ON" o "OFF" en el feed de Adafruit IO
3. El Arduino recibe el mensaje y enciende el LED correspondiente


### Raspberry Pi Pico 2W

- Cada vez que presionas el botón, alterna entre ON y OFF (no es necesario mantenerlo)
- El LED integrado de la placa te muestra el estado actual (encendido = ON, apagado = OFF)
- Publica el texto "ON" o "OFF" en el feed estado de Adafruit IO

### Arduino UNO R4 WiFi

- Recibe "ON" → enciende LED verde (D2), apaga el rojo
- Recibe "OFF" → enciende LED rojo (D3), apaga el verde
- Al arrancar, ambos LEDs parpadean 3 veces para confirmar que la conexión fue exitosa

`Recordar!!`

1. TU_NOMBRE_WIFI / TU_CLAVE_WIFI
2. TU_USUARIO_ADAFRUIT / TU_AIO_KEY
3. En el .ino, también reemplaza TU_USUARIO_ADAFRUIT en la línea del feed
4. Crear el feed llamado estado en tu cuenta de Adafruit IO antes de ejecutar
