[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21148383&assignment_repo_type=AssignmentRepo)
# Proyecto integrador 1ra Entrega

## Integrantes
Michael Yesid Velasquez V.- Cod: 94882 Yeison Gabriel Niño J. - Cod: 61096 Carlos Eduardo Puentes L. - Cod: 89466


## Arquitectura propuesta




## Periférico a trabajar
Resistencia, Para calentar las pinturas
## Avances

import network
import time
from machine import Pin, I2C
from umqtt.robust import MQTTClient

# Importar configuración desde config.py
try:
    from config import WIFI_SSID, WIFI_PASS, MQTT_BROKER, MQTT_TOPIC
except ImportError:
    raise Exception("⚠️ Debes crear un archivo config.py con tus credenciales (ver config.py.sample)")

# =======================
# Configuración I2C y LED
# =======================
i2c = I2C(0, scl=Pin(22), sda=Pin(21), freq=100000)
led = Pin(13, Pin.OUT)
led.off()

# =======================
# Umbrales de temperatura
# =======================
TEMPERATURA_ENCENDER = 29.0  # Encender LED cuando supere 29°C
TEMPERATURA_APAGAR  = 28.0  # Apagar LED cuando baje de 28°C

# =======================
# Conectar WiFi
# =======================
def conectar_wifi():
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(WIFI_SSID, WIFI_PASS)
    print("Conectando a WiFi...", end="")
    while not wlan.isconnected():
        print(".", end="")
        time.sleep(1)
    print("\n✅ WiFi conectado:", wlan.ifconfig())

# =======================
# Conectar MQTT
# =======================
def conectar_mqtt():
    client = MQTTClient("esp32", MQTT_BROKER)
    client.connect()
    print("✅ Conectado a broker MQTT:", MQTT_BROKER)
    return client

# =======================
# Simulación lectura temperatura
# (En práctica puedes leer desde un sensor real como LM75, DHT, etc.)
# =======================
def leer_temperatura():
    # Aquí puedes poner lectura real del sensor
    return 25 + (time.time() % 10)  # Simulación 25°C a 35°C

# =======================
# Programa principal
# =======================
def main():
    conectar_wifi()
    client = conectar_mqtt()

    estado_led = False

    try:
        while True:
            temp = leer_temperatura()
            print(f"🌡️ Temperatura: {temp:.2f} °C")

            # Publicar al broker
            client.publish(MQTT_TOPIC, str(temp))

            # Control LED según umbrales
            if temp >= TEMPERATURA_ENCENDER and not estado_led:
                led.on()
                estado_led = True
                print("💡 LED encendido")
            elif temp <= TEMPERATURA_APAGAR and estado_led:
                led.off()
                estado_led = False
                print("💡 LED apagado")

            time.sleep(2)

    except KeyboardInterrupt:
        print("🛑 Programa interrumpido")
        client.disconnect()

if __name__ == "__main__":
    main()


# Variable para controlar el estado actual d
<!-- Subir en una carpeta src los códigos que tienen hasta el momento y esta sección agregar lo que consideren necesario referente a sus avances. -->
