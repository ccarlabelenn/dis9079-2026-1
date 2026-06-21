# ⋆⭒˚.⋆ └[∵┌] Examen "Grupo 02" - Acu-Visual [┐∵]┘ ⋆.˚⭒⋆

Lunes 22 de junio 2026

---

## Grupo 02. Pa-Pa's

### Integrantes

* [Camila Parada](https://github.com/Camila-Parada): Código, Investigación, Shopper, Tester
* [Vania Paredes](https://github.com/paredesvania): Touchdesigner, Código, Proyección, Registro.

## Descripción del proyecto

### _"¿Cómo presenciamos el "habitar" de los espacios a través del sonido presente en los edificios de la FAAD?"_

Nos interesa observar en vivo las huellas sonoras (conversaciones, pasos, risas, silencios, etc) que dejan las personas al ocupar o transitar un lugar (espacio físico). Esta "identidad acústica" cambiante nos habla de cómo se vive y se comparte un espacio . Estos registros en tiempo real son la materia prima para la producción de visualizaciones experimentales producidas en Touchdesigner,

La dimensión material del proyecto abarca el uso de 2 placas rapsberry pi pico 2W, cada una con un "Sensor de Sonido (LM393)" que reúne información y la sube en 2 feeds en Adafruit IO. Cada uno de estos módulos se encuentran ubicados en distintos pisos de la Facultad de Artes, Arquitectura y Diseño Salvador Sanfuentes 2221, un micrófono ubicado en pañol y el otro en el Laboratorio de interacción digital (LID).

Por otra parte, el computador (o el Arduino) recibirá dichos datos para posteriormente entregarlos a Touchdesigner. La visualización generativa en tiempo real posee variables como el movimiento, las formas y los colores que responden a la actividad sonora de cada lugar.

De esta manera, aquello que normalmente percibimos solo con el oído podrá manifestarse visualmente frente a nosotros.

Buscamos hacer visible una dimensión cotidiana que suele pasar desapercibida: la manera en que habitamos los espacios y cómo nuestra presencia los transforma a través de la relación entre sonido e imagen, la visualización funcionará como un retrato vivo de ambos lugares.

## Pseudocódigo Raspberry (input)

1. Conectar Raspberry Pi Pico a WiFi
2. Conectar Raspberry Pi Pico a Adafruit IO mediante MQTT
-  MIENTRAS el sistema esté funcionando
3. Leer datos del micrófono
4. Calcular nivel de sonido
5. Convertir nivel de sonido a porcentaje (0% a 100%)
6. Enviar porcentaje al feed MQTT en adafruit

## Pseudocódigo Touchdesigner (output)

1. Conectarse a Adafruit IO mediante MQTT
2. Suscribirse a los feeds de ambos lugares
- CUANDO llegue un mensaje
3. Leer valor recibido
4. Identificar de qué lugar proviene
5. Actualizar la variable correspondiente
6. Mostrar el dato en la visualización

## Primeros acercamientos

En un inicio se utilizaron varias

## Input: Micrófono 

Para comenzar...

### Código Micrófono

```cpp
# ============================================================
# SENSOR DE SONIDO - Raspberry Pi Pico 2W + MAX9812
# Examen interacciones inalambricas
# ============================================================
# CONEXIONES MAX9812:
#   VCC  -> Pin 36 (3V3)
#   GND  -> Pin 38 (GND)
#   OUT  -> Pin 31 (GP26)
# ============================================================

import time
import board
import analogio
import wifi
import socketpool
import adafruit_minimqtt.adafruit_minimqtt as MQTT

# ============================================================
# CONFIGURACION - esto es lo unico que cambia entre las dos Raspberry
# ============================================================

EDIFICIO      = "grupo02-rep"   # nombre del feed, cambia segun el edificio
WIFI_SSID     = "Nombre wifi"
WIFI_PASSWORD = "01234"

AIO_USERNAME  = "udpmontoyamoraga"
AIO_KEY       = "aio_secret"

# ============================================================
# PARAMETROS DE MEDICION
# ============================================================

RUIDO_PISO   = 150    # bajo este numero se considera silencio
AMPLITUD_MAX = 5000   # esto es lo que se considera 100%

NUM_MUESTRAS = 150    # cuantas lecturas toma por rafaga
INTERVALO_S  = 1.0    # cada cuanto envia datos

# ============================================================
# SENSOR
# ============================================================

PINES_SENSORES = [board.GP26]
sensores       = [analogio.AnalogIn(pin) for pin in PINES_SENSORES]

# ============================================================
# RED
# ============================================================

pool         = None
mqtt_cliente = None


def estado_wifi():
    # chequea si seguimos conectados
    try:
        return wifi.radio.connected
    except Exception:
        return False


def conectar_wifi():
    print(f"Conectando a wifi: '{WIFI_SSID}'")
    while not estado_wifi():
        try:
            wifi.radio.connect(WIFI_SSID, WIFI_PASSWORD)
            time.sleep(1)
            if estado_wifi():
                print(f"  conectado - IP: {wifi.radio.ipv4_address}")
                return
        except Exception as e:
            print(f"  fallo: {e}")
        print("  reintentando en 5s...")
        time.sleep(5)


def crear_mqtt():
    # arma una conexion mqtt nueva desde cero
    global pool, mqtt_cliente
    pool = socketpool.SocketPool(wifi.radio)
    mqtt_cliente = MQTT.MQTT(
        broker         = "io.adafruit.com",
        port           = 1883,
        username       = AIO_USERNAME,
        password       = AIO_KEY,
        socket_pool    = pool,
        socket_timeout = 1,
        keep_alive     = 30,
    )


def conectar_mqtt():
    intentos = 0
    while True:
        try:
            print("Conectando a adafruit io...")
            mqtt_cliente.connect()
            print(f"  listo - feed: {EDIFICIO}")
            return
        except Exception as e:
            intentos += 1
            espera = min(3 * intentos, 30)
            print(f"  fallo: {e}. reintentando en {espera}s...")
            try:
                mqtt_cliente.disconnect()
            except Exception:
                pass
            time.sleep(espera)


def asegurar_conexiones():
    # revisa que todo siga vivo, y si se cayo algo, lo reconstruye
    if not estado_wifi():
        print("wifi caido. reconectando todo...")
        try:
            mqtt_cliente.disconnect()
        except Exception:
            pass
        conectar_wifi()
        crear_mqtt()
        conectar_mqtt()
        return
    try:
        mqtt_cliente.loop(timeout=1)
    except Exception as e:
        print(f"mqtt caido: {e}. reconectando...")
        try:
            mqtt_cliente.disconnect()
        except Exception:
            pass
        if not estado_wifi():
            conectar_wifi()
            crear_mqtt()
        conectar_mqtt()


def publicar(valor):
    # manda el dato al feed de adafruit io
    try:
        asegurar_conexiones()
        mqtt_cliente.publish(f"{AIO_USERNAME}/feeds/{EDIFICIO}", str(valor))
        print(f"  -> {EDIFICIO}: {valor}%")
    except Exception as e:
        print(f"  error: {e}")
        try:
            mqtt_cliente.disconnect()
        except Exception:
            pass
        conectar_wifi()
        crear_mqtt()
        conectar_mqtt()

# ============================================================
# MEDICION - toma el nivel mas alto
# ============================================================

def medir_nivel():
    """
    recorre el arreglo de sensores.
    en cada uno toma una rafaga rapida de 150 lecturas y se queda
    con la diferencia entre el valor mas alto y el mas bajo.
    eso es lo que indica que tan fuerte sono algo.
    """
    niveles = []

    for i in range(len(sensores)):

        # rafaga rapida, sin pausas entre lecturas
        maximo = 0
        minimo = 65535
        for j in range(NUM_MUESTRAS):
            v = sensores[i].value
            if v > maximo:
                maximo = v
            if v < minimo:
                minimo = v

        amplitud = maximo - minimo

        if amplitud < RUIDO_PISO:
            porcentaje = 0
        else:
            porcentaje = int(((amplitud - RUIDO_PISO) / AMPLITUD_MAX) * 100)
            porcentaje = max(0, min(100, porcentaje))

        niveles.append(porcentaje)

    return max(niveles)

# ============================================================
# INICIO
# ============================================================

conectar_wifi()
crear_mqtt()
conectar_mqtt()

ultimo_envio   = 0
nivel_maximo   = 0   # guarda el sonido mas fuerte detectado desde el ultimo envio

print(f"\n=== [{EDIFICIO.upper()}] escuchando... ===\n")

# ============================================================
# LOOP PRINCIPAL
# ============================================================

while True:
    try:
        # mide todo el tiempo, sin parar
        nivel = medir_nivel()

        # si este nivel es mas fuerte que el guardado, lo reemplaza
        if nivel > nivel_maximo:
            nivel_maximo = nivel
            print(f"    nuevo maximo detectado: {nivel_maximo}%")

        # cada 2 segundos manda el nivel mas fuerte que pillo
        ahora = time.monotonic()
        if (ahora - ultimo_envio) >= INTERVALO_S:
            publicar(nivel_maximo)
            nivel_maximo = 0   # se resetea para el siguiente ciclo
            ultimo_envio = ahora

    except Exception as e:
        print(f"error: {e}. reconectando...")
        try:
            mqtt_cliente.disconnect()
        except Exception:
            pass
        conectar_wifi()
        crear_mqtt()
        conectar_mqtt()
        time.sleep(2)
```

## Output: Touchdesigner

## Codigo Mqtt client / touch designer

```python
# mqttclient1_callbacks

def onConnect(dat, *args):
    dat.subscribe('udpmontoyamoraga/feeds/grupo02-rep')
    dat.subscribe('udpmontoyamoraga/feeds/grupo02-ss')
    print('MQTT conectado')
    return

def onDisconnect(dat, *args):
    print('MQTT desconectado')
    return

def onMessage(dat, topic, payload, qos, retain):
    try:
        valor = float(payload)
        valor = max(0.0, min(100.0, valor))
    except:
        return

    if 'rep180' in topic:
        op('constant_rep').par.value0 = valor
        print(f'[rep180] {valor:.0f}%')

    elif topic.endswith('/ss'):
        op('constant_ss').par.value0 = valor
        print(f'[ss]     {valor:.0f}%')
    return

def onSubscribe(dat, *args):
    print(f'Suscrito a: {args}')
    return

def onUnsubscribe(dat, *args):
    return
```

## Parámetros del mqtt_client DAT

![foto](./imagenes/voice14.png)

## Demostraciones en vivo

### Video en el mismo edificio (distintos espacios)

Este video fué grabado con uno de los micrófonos en el Laboratorio de interacción digital, otro en pañol y el output en la sala 102 de Salvador Sanfuentes 2221, en el proyector de la sala. Todo en el mismo edificio.

[![Ver video](https://img.youtube.com/vi/wxTEaNKYboA/maxresdefault.jpg)](https://youtube.com/shorts/wxTEaNKYboA)

[![Ver video](https://img.youtube.com/vi/n-fH_hPftp4/maxresdefault.jpg)](https://youtu.be/n-fH_hPftp4)

---

## Bill of materials (listado de materiales)

| Componentes         | Tipo  | Cantidad | Precio  | Enlace            |
| ------------------- | ----- | -------- | ------- | ----------------  |
| Raspberry Pi Pico 2 W | Placa de desarrollo | 2   | $14.990 | <https://mcielectronics.cl/shop/product/74358//> |
| Mini Protoboard 400 Puntos | Placa prototipado | 2  | $1.500 | <https://afel.cl/products/mini-protoboard-400-puntos> |
| Cable Dupont Macho Macho 10cm | Cable | Pack 40 | $2.590 | <https://mcielectronics.cl/shop/product/cable-dupont-macho-macho-20cm-pack-40-unidades/> |
| Sensor Analógico Sonido/Audio MAX9812 | Sensor | 1 | $3.790 | <https://hubot.cl/producto/sensor-analogico-audio-max9812-sku-614/> |
| Pantalla LCD OLED 0,96 | Componente | 1 | $4.500 | <https://afel.cl/products/pantalla-lcd-oled-azul-y-amarillo-0-96> |

---

## Mapa de flujo


```mermaid
flowchart TD

A[Micrófono en el LID] --> B[Raspberry Pi Pico 2W]
C[Micrófono en pañol] --> D[Raspberry Pi Pico 2W]

B --> E[Procesar nivel de sonido]
D --> F[Procesar nivel de sonido]

E --> G[Enviar datos por MQTT]
F --> G

G --> H[Adafruit IO]

H --> I[TouchDesigner]

I --> J[Recibir datos de REP180]
I --> K[Recibir datos de SS]

J --> L[Actualizar visualización]
K --> L
```

## Investigaciones individuales

Aportes, información y exploraciones personales compartidas con el equipo.

- [Camila Parada.md](./persona-01.md) 

- [Vania Paredes.md](./persona-02.md)

## Bibliografía

* <https://learn.adafruit.com/series/adafruit-io-basics>
