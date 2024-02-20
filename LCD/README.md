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
[]()
