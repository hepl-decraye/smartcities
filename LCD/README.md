# EXERCICE 3 : SYSTÈME DE CONTRÔLE DE TEMPÉRATURE 
## Objectif
Créer un programme MicroPython qui permet gérer un thermostat à plusieurs états.
## Matériel
• Microcontrôleur compatible MicroPython (Raspberry Pi Pico)\
• Module capteur température/humidité\
• Module LED\
• Module potentiomètre\
• Module écran LCD\
• Module Buzzer\
• Câbles
## Consignes
1. Développez un programme en MicroPython sur le Raspberry Pico W pour: \
  o Lire la valeur de la résistance variable et la convertir en une température de consigne
    dans une plage de 15°C à 35 °C.\
  o Lire la température mesurée par le capteur toutes les secondes environ.\
  o Comparer la température mesurée à la température de consigne.\
  o Afficher sur le module LCD: 
      - La température de consigne, préfixée par "Set: ". 
      - La température mesurée préfixée par "Ambient: ". 
      
      o Contrôle:
      - Si la température mesurée est supérieure à la température de consigne: 
      - La LED bat à une fréquence de 0,5 Hz.
      - Si la température mesurée est supérieure de 3 degrés à la température de \
        consigne: 
          - Le buzzer sonne.
          - La LED clignote plus rapidement.
          - Le mot "ALARM" apparait sur l'écran LCD.
## Bonus
• Afficher un battement progressif (dimmer) de la LED.\
• Faire clignoter le mot "ALARM" à l'écran.\
• Faire défiler le mot "ALARM" sur l'écran.
## Introduction
<p align = justify>
Ce rapport présente la réalisation d'un programme en MicroPython sur le Raspberry Pi Pico, visant à mettre en place un système de contrôle de température. Pour ce faire, nous avons utilisé divers composants tels que le capteur de température/humidité, la LED, le potentiomètre, l'écran LCD, et le Buzzer. L'objectif principal est de lire la valeur du potentiomètre, convertir cette valeur en une température de consigne, surveiller la température ambiante en temps réel, et afficher ces données sur l'écran LCD. Le programme inclut des fonctionnalités de contrôle, telles que l'activation de la LED et l'émission d'une alarme en cas de dépassement de la température de consigne. Des fonctionnalités bonus, comme le battement progressif de la LED et le clignotement/défilement de l'alarme à l'écran on été ajouté. 
</p>

## Câblage
Pour ce projet plusieurs modules ont été intégré au rapsberry pico : potentiomètre, led, buzzer, capteur DHT11 et l'écran lcd pour l'affichage (voir figure).
<p align = center>
<img src="https://github.com/hepl-decraye/smartcities/blob/main/images/Image5.png">
</p>

## Explication du code
### Importation des modules nécessaires :
```Python
from lcd1602 import LCD1602
import dht
from machine import I2C, Pin, ADC, PWM
from utime import sleep
```

### Configuration des broches pour le buzzer, le potentiomètre, la LED, l'I2C (pour l'écran LCD) et le capteur DHT11 :
- La première ligne configure le buzzer comme une sortie PWM sur la broche 27.
- La deuxième ligne configure le potentiomètre comme une broche analogique sur la broche 0.
- La troisième ligne configure la broche 20 comme une sortie pour la LED.
- Les lignes suivantes initialisent la communication I2C pour l'écran LCD 1602.
- La dernière ligne initialise le capteur DHT11 sur la broche 18.
```Python
buzzer = PWM(Pin(27))
potentiometer_pin = ADC(0)
led_pin = Pin(20, Pin.OUT)
i2c = I2C(1, scl=Pin(7), sda=Pin(6), freq=400000)
d = LCD1602(i2c, 2, 16)
d.display()
dht_sensor = dht.DHT11(machine.Pin(18))
```

### Fonction pour lire la valeur du potentiomètre :
La fonction lit la valeur du potentiomètre entre 0 et 65535, puis renvoie une valeur comprise dans une plage de 15 à 35.
```Python
def read_potentiometer():
    pot_value = potentiometer_pin.read_u16()
    return 15 + (pot_value / 65535) * 20
```

### Fonction pour lire la température du capteur DHT11 :
Cette fonction mesure la température à l'aide du capteur DHT11 et renvoie la valeur mesurée.
```Python
def read_temperature():
    dht_sensor.measure()
    temp = dht_sensor.temperature()
    return temp
```

### Fonction pour contrôler la température et activer le buzzer
Cette fonction compare la température mesurée à la température de consigne. Si la température mesurée est supérieure de 3 degrés, elle affiche une alarme sur l'écran LCD, active le buzzer et efface ensuite l'alarme après une courte période pour qu'elle clignote. Sinon, elle désactive le buzzer.
```Python
def control_temperature(set_temperature, measured_temperature):
    if measured_temperature > set_temperature + 3:
        d.setCursor(4, 0)
        d.print("!!ALARM!!")
        sleep(0.5)
        d.setCursor(4, 0)
        d.print("               ")
        buzzer.freq(500)
        buzzer.duty_u16(600)
    else:
        d.print("")
        buzzer.freq(600)
        buzzer.duty_u16(0)
```

### Boucle principale
La boucle principale lit en continu la valeur du potentiomètre, la température mesurée, affiche ces valeurs sur l'écran LCD, appelle la fonction `control_temperature`, et fait clignoter la LED si la température mesurée est supérieure à la température de consigne. 
```Python
while True:
    set_temperature = read_potentiometer()
    measured_temperature = read_temperature()
    d.setCursor(7, 1)
    d.print("S: " + str(set_temperature))
    d.setCursor(0, 1)
    d.print("A: " + str(measured_temperature))
    control_temperature(set_temperature, measured_temperature)

    if measured_temperature > set_temperature:
        led_pin.value(0)
        sleep(0.5)
        led_pin.value(1)
        sleep(0.5)
    else:
        led_pin.value(0)
        sleep(1)
```

## Code complet
```Python
from lcd1602 import LCD1602
import dht
from machine import I2C,Pin,ADC,PWM
from utime import sleep
 
buzzer = PWM(Pin(27))
potentiometer_pin = ADC(0) 
led_pin = Pin(20, Pin.OUT) 
 
i2c = I2C(1,scl=Pin(7), sda=Pin(6), freq=400000)
d = LCD1602(i2c, 2, 16)
d.display()
dht = dht.DHT11(machine.Pin(18))

def read_potentiometer():
    pot_value = potentiometer_pin.read_u16()
    return 15 + (pot_value / 65535) * 20 

def read_temperature():
    dht.measure()
    temp = dht.temperature()
    return temp

def control_temperature(set_temperature, measured_temperature):
        
    if measured_temperature > set_temperature + 3:
       # d.clear()
        d.setCursor(4, 0)
        d.print("!!ALARM!!")
        sleep(0.5)
        d.setCursor(4, 0)
        d.print("               ")

        buzzer.freq(500)
        buzzer.duty_u16(600)
                   
    else:
        d.print("")
        buzzer.freq(600)
        buzzer.duty_u16(0)

while True:
    set_temperature = read_potentiometer()
    measured_temperature = read_temperature()
    d.setCursor(7, 1)
    d.print("S: "+str(set_temperature))
    d.setCursor(0, 1)
    d.print("A: "+str(measured_temperature))
    control_temperature(set_temperature, measured_temperature)
    
    if measured_temperature > set_temperature:
        led_pin.value(0)
        sleep(0.5)
        led_pin.value(1)
        sleep(0.5)
        
    else:
        led_pin.value(0)
        sleep(1)
```

## Test du programme


https://github.com/hepl-decraye/smartcities/assets/159047970/cce2f0df-c521-408a-bd7a-de8e8c218507


## Conclusion


