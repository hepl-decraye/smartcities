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
import machine
import utime

# Définir les broches pour le potentiomètre, le buzzer, le bouton poussoir et la LED
potentiometer_pin = 26
buzzer_pin = 18
button_pin = 14  # Choisissez une broche GPIO disponible pour le bouton poussoir
led_pin = 13     # Choisissez une broche GPIO disponible pour la LED

# Initialiser les objets ADC, PWM, et Pin
adc = machine.ADC(machine.Pin(potentiometer_pin))
pwm = machine.PWM(machine.Pin(buzzer_pin))
button = machine.Pin(16, machine.Pin.IN)
led = machine.Pin(20, machine.Pin.OUT)

# Fonction pour lire la valeur du potentiomètre
def read_potentiometer():
    return adc.read_u16()

# Fonction pour jouer une mélodie avec un certain volume
def play_melody(volume, melody):
    for note_freq in melody:
        pwm.freq(note_freq)
        pwm.duty_u16(int(volume))
        utime.sleep(0.5)
        pwm.duty_u16(0)

# Définir deux mélodies pour illustrer le changement
melody1 = [440, 523, 659, 784, 880, 987, 1046]
melody2 = [523, 587, 659, 784, 880, 988, 1175]

current_melody = melody1  # Mélodie par défaut
button_pressed = False    # État initial du bouton

# Boucle principale
while True:
    pot_value = read_potentiometer()
    
    # Convertir la valeur du potentiomètre en plage de volume (par exemple, 0-65535)
    volume = pot_value / 65535.0 * 2530
    
    # Vérifier l'état du bouton poussoir
    if button.value() == 0:  # Le bouton est enfoncé (peut dépendre de la connexion physique)
        if not button_pressed:
            print("Bouton enfoncé !")
            # Changer de mélodie
            if current_melody == melody1:
                current_melody = melody2
            else:
                current_melody = melody1
            button_pressed = True  # Mettre à jour l'état du bouton
    else:
        button_pressed = False  # Réinitialiser l'état du bouton
        
    # Jouer la mélodie avec le volume actuel du potentiomètre
    play_melody(volume, current_melody)
    
    # Allumer la LED pendant que le bouton est enfoncé
    if button_pressed:
        led.on()
    else:
        led.off()


```
