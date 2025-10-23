[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21148383&assignment_repo_type=AssignmentRepo)
# Proyecto integrador 1ra Entrega

## Integrantes
Michael Yesid Velasquez V.- Cod: 94882 Yeison Gabriel Niño J. - Cod: 61096 Carlos Eduardo Puentes L. - Cod: 89466


## Arquitectura propuesta

<img width="724" height="485" alt="image" src="https://github.com/user-attachments/assets/c9f52c45-db2b-4ed0-9eb3-ed5729e8185e" />



## Periférico a trabajar
Resistencia, Para calentar pintura.
## Avances
![Imagen de WhatsApp 2025-10-22 a las 15 22 53_9813038a](https://github.com/user-attachments/assets/4aa345fb-5db2-462f-85b4-7623e04b3f49)
![Imagen de WhatsApp 2025-10-22 a las 15 22 45_78267179](https://github.com/user-attachments/assets/e41c3b74-b8df-4034-8dc9-d05689e34b05)
![Imagen de WhatsApp 2025-10-22 a las 15 22 27_a10c3af5](https://github.com/user-attachments/assets/80afded1-4d9d-4a3f-a1c3-6e94ee09d1ee)
![Imagen de WhatsApp 2025-10-22 a las 15 22 27_8f121f36](https://github.com/user-attachments/assets/98fbf53b-1913-4f66-8611-3dd2fbb493b8)
![Imagen de WhatsApp 2025-10-22 a las 15 22 26_e0717ff6](https://github.com/user-attachments/assets/856f7239-9ca9-40cf-bfa4-eeca4ae844fd)
![Imagen de WhatsApp 2025-10-22 a las 15 22 25_f4978d3c](https://github.com/user-attachments/assets/1090b54f-73f7-4983-96c0-66be65da83bf)


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

<!-- Subir en una carpeta src los códigos que tienen hasta el momento y esta sección agregar lo que consideren necesario referente a sus avances. -->
