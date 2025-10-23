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

# =======================
# CONFIGURACIONES
# =======================
WIFI_SSID = "iPhone Michael"   # Cambia por tu red WiFi
WIFI_PASS = "michael96"              # Contraseña de tu WiFi
MQTT_BROKER = "172.20.10.2"       # IP de la Raspberry Pi (Broker MQTT)
MQTT_TOPIC = b"in/micro/sensor/temperatura"   # Tema donde se publicarán los datos

# Pines I2C ESP32
i2c = I2C(0, scl=Pin(22), sda=Pin(21), freq=100000)

# LED en pin 8/6. 
led = Pin(13, Pin.OUT)
led.off()  # Asegurar que el LED empiece apagado

# Umbrales de temperatura
TEMPERATURA_ENCENDER = 10.0  # Encender LED cuando sea mayor a 20°C
TEMPERATURA_APAGAR = 25.0    # Apagar LED cuando supere 30°C

# Variable para controlar el estado actual d

<!-- Subir en una carpeta src los códigos que tienen hasta el momento y esta sección agregar lo que consideren necesario referente a sus avances. -->
