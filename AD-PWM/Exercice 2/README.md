# EXERCICE 2 : VOLUME D'UNE MÉLODIE 
## Objectif
Créer un programme MicroPython permet de gérer le volume d’une mélodie jouée sur un buzzer. Le 
volume est contrôlé par un potentiomètre.
## Matériel
• Microcontrôleur compatible MicroPython (Raspberry Pi Pico) \
• Module potentiomètre \
• Buzzer \
• Câbles 
## Consignes
1. Branchez le buzzer et le potentiomètre au microcontrôleur 
2. Ecrivez un programme MicroPython qui répond aux exigences suivantes :  \
o Une mélodie (au choix, soyez créatif) est jouée en boucle. \
o Le faite de changer le potentiomètre modifie directement le volume de la mélodie. 
3. Testez votre programme et vérifiez qu'il fonctionne correctement.
## Bonus
• Ajoutez un bouton poussoir qui permet de changer de mélodie. \
• Ajouter une LED qui clignote au rythme de la mélodie.
## Introduction

## Cablage
![]()
## Le code
```
from machine import Pin, PWM, ADC
import time

# Définir les broches utilisées
buzzer_pin = 18  # Broche pour le buzzer
potentiometer_pin = 26  # Broche pour le potentiomètre
button_pin = 16  # Broche pour le bouton poussoir
led_pin = 20  # Broche pour la LED

# Initialiser les composants
buzzer = PWM(Pin(buzzer_pin))
potentiometer = ADC(Pin(potentiometer_pin))
button = Pin(button_pin, Pin.IN, Pin.PULL_DOWN)
led = Pin(led_pin, Pin.OUT)

# Liste des mélodies (à personnaliser)
melodies = [
    [262, 294, 330, 349, 392, 440, 494, 523],  # Mélodie 1
    [555, 444, 333, 222, 555, 666, 800, 587]   # Mélodie 2 (exemple)
]

# Indice de la mélodie en cours
current_melody = 0

# Indice de la note en cours dans la mélodie
current_note = 0

# Fonction pour jouer la mélodie avec le volume du potentiomètre
def play_melody(volume):
    global current_note
    buzzer.freq(melodies[current_melody][current_note])
    buzzer.duty_u16(int(5535 * volume / 100))
    current_note = (current_note + 1) % len(melodies[current_melody])

# Fonction de gestion de l'interruption du bouton poussoir
def button_callback(pin):
    global current_melody
    current_melody = (current_melody + 1) % len(melodies)

# Configuration de l'interruption pour le bouton poussoir
button.irq(trigger=Pin.IRQ_RISING, handler=button_callback)

# Boucle principale
while True:
    volume = potentiometer.read_u16() * 100 / 65535  # Calculer le volume en pourcentage
    play_melody(volume)

    # Clignoter la LED au rythme de la mélodie
    led.value(1)
    time.sleep(0.1)
    led.value(0)
    time.sleep(0.1)

```
