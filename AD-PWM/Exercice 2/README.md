# EXERCICE 2 : VOLUME D'UNE MÉLODIE 
## Objectif
Créer un programme MicroPython qui permet de gérer le volume d’une mélodie jouée sur un buzzer. Le 
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
<p align="justify">
Pour ce projet, j'ai créé un code qui permet de jouer une mélodie sur un buzzer, avec une LED clignotante synchronisée à la musique. J'ai intégré un bouton poussoir pour changer la mélodie et un potentiomètre pour ajuster le volume en temps réel. Pour rendre le code plus efficace, j'utilise le bouton en mode interruption.
</p>

## Câblage
Pour cet exercice, j'ai choisi de câbler le potentiomètre sur la broche A0, le bouton sur la broche D16, le buzzer sur la broche D18 et la LED sur la broche D20.
<p align="center">
  <img src="https://github.com/hepl-decraye/smartcities/blob/main/images/Image2.png">
</p>

## Le code
### Importation des bibliothèques
```Python
from machine import Pin, PWM, ADC
from time import sleep
```
### Fonction pour jouer une note
La fonction `play_note` est définie pour jouer une note sur le buzzer. Elle règle la fréquence et le cycle de service du buzzer PWM en fonction de la note et de la durée. Si `note` est None, cela représente une pause (silence).
```Python
def play_note(note, duration, vol):
    # Fonction pour jouer une note sur le buzzer
    if note is None:
        buzzer.duty_u16(0)  # Silence
    else:
        led_pin.toggle()  # Inverse l'état d'une LED
        buzzer.freq(note)
        buzzer.duty_u16(int(vol))
        sleep(duration)
```
### Fonction pour l'interruption du bouton
Cette fonction est appelée lorsqu'une interruption de bouton (<strong>front descendant</strong>) se produit sur la broche 16. Elle alterne entre deux mélodies (melody_1 et melody_2).
```Python
def button_interrupt_handler(pin):
    global current_melody
    if current_melody == melody_1:
        current_melody = melody_2
    else:
        current_melody = melody_1
```
### Configuration de la broche du bouton et de l'interruption
Configure la broche 16 en entrée avec une résistance de tirage vers le bas et met en place une interruption pour appeler button_interrupt_handler sur un front descendant.
```Python
button_pin = Pin(16, Pin.IN, Pin.PULL_DOWN)
button_pin.irq(trigger=Pin.IRQ_FALLING, handler=button_interrupt_handler)
```
### Définition des notes
Définition de fonctions telles que DO, RE, ..., DO4, RE4, ... représentant la fréquence et la durée de la note.\
Voir le code complet pour toutes les notes.
```Python
#EXEMPLE
def LA(time): 
    return 440, time
```

### Initialisations
Initialise la PWM pour le buzzer, l'ADC pour le potentiomètre, et configure une LED sur la broche 20 en sortie.
```Python
buzzer = PWM(Pin(27))
potentiometer_pin = ADC(Pin(26))
led_pin = Pin(20, Pin.OUT)
```

### Définition des mélodies
Deux mélodies (melody_1 et melody_2) sont définies en utilisant les fonctions de note avec des durées spécifiées.
```Python
melody_1 = [ MI(0.41), NI(0.2), MI(0.47), NI(0.2), MI4(0.42), NI(0.2), RE4(0.42), NI(0.2), DO4(0.42), NI(0.2), SO4(1), NI(0.2),
    SO4(0.3), NI(0.2), FA4(0.22), NI(0.2), FA4(0.22), NI(0.2), MI4(0.22), NI(0.2), MI4(0.22), NI(0.2), RE4(1), NI(0.2),
    RE4(0.3), NI(0.2), DO4(0.22), NI(0.2), MI(0.22), NI(0.2), MI(0.22), NI(0.2), MI4(0.22), NI(0.2), RE4(0.54), NI(0.2),
    SO4(0.22), NI(0.2), SO4(0.22), NI(0.2), FA4(0.22), NI(1),]
melody_2 = [DO(0.25), NI(0.05), RE(0.25), NI(0.05), MI(0.25), NI(0.05), DO(0.25), NI(0.05), NI(0.01), DO(0.25), NI(0.05), RE(0.25), NI(0.05),
           MI(0.25), NI(0.05), DO(0.25), NI(0.05), MI(0.25), NI(0.05), FA(0.25), NI(0.05), SO(0.5), NI(0.05),
           MI(0.25), NI(0.05), FA(0.25), NI(0.05), SO(0.5), NI(0.05), NI(0.01),
           SO(0.125), NI(0.05), LA(0.125), NI(0.05), SO(0.125), NI(0.05), FA(0.125), NI(0.05), MI(0.25), NI(0.05), DO(0.25), NI(0.05),
           SO(0.125), NI(0.05), LA(0.125), NI(0.05), SO(0.125), NI(0.05), FA(0.125), NI(0.05),
           MI(0.25), NI(0.05), DO(0.25), NI(0.05), RE(0.25), NI(0.05), SO(0.25), NI(0.05),
           DO(0.5), NI(0.05), NI(0.01),RE(0.25), NI(0.05), SO(0.25), NI(0.05), DO(0.5)]
```
### Boucle principale
Dans une boucle infinie,le code lit la valeur du potentiomètre, ajuste le volume, joue la note actuelle, puis passe à la note suivante dans la mélodie. La mélodie est jouée en boucle continue.
```Python
while True:
    # Lecture de la valeur du potentiomètre en temps réel
    pot_value = potentiometer_pin.read_u16()
    vol = int((pot_value / 65535) * 1023)

    # Joue la note avec le volume adapté
    note, duration = current_melody[0]
    play_note(note, duration, vol)

    # Change la note pour l'itération suivante
    current_melody.append(current_melody.pop(0))

```
### Le code complet
```Python
from machine import Pin, PWM, ADC
from time import sleep

def play_note(note, duration, vol):
    if note is None:
        buzzer.duty_u16(0)  # Silence
    else:
        led_pin.toggle()
        buzzer.freq(note)
        buzzer.duty_u16(int(vol))
        sleep(duration)
        
def button_interrupt_handler(pin):
    global current_melody
    if current_melody == melody_1:
        current_melody = melody_2
    else:
        current_melody = melody_1

button_pin = Pin(16, Pin.IN, Pin.PULL_DOWN)
button_pin.irq(trigger=Pin.IRQ_FALLING, handler=button_interrupt_handler)  

# Definition des notes
def DO(time):
    return 262, time
def RE(time):
    return 294, time
def MI(time):
    return 330, time
def FA(time):
    return 350, time
def SO(time):
    return 392, time
def LA(time):
    return 440, time
def SI(time):
    return 494, time
def NI(time):
    return 15000, time
#_____________________
def DO4(time):
    return 523, time
def RE4(time):
    return 587, time
def MI4(time):
    return 660, time
def FA4(time):
    return 699, time
def SO4(time):
    return 784, time
def LA4(time):
    return 880, time
def SI4(time):
    return 1967, time

# Initialisation
buzzer = PWM(Pin(27))
potentiometer_pin = ADC(Pin(26)) 
led_pin = Pin(20, Pin.OUT)  

melody_1 = [
    MI(0.41), NI(0.2), MI(0.47), NI(0.2), MI4(0.42), NI(0.2),
    RE4(0.42), NI(0.2), DO4(0.42), NI(0.2), SO4(1), NI(0.2),
    SO4(0.3), NI(0.2), FA4(0.22), NI(0.2), FA4(0.22), NI(0.2),
    MI4(0.22), NI(0.2), MI4(0.22), NI(0.2), RE4(1), NI(0.2),
    RE4(0.3), NI(0.2), DO4(0.22), NI(0.2), MI(0.22), NI(0.2),
    MI(0.22), NI(0.2), MI4(0.22), NI(0.2), RE4(0.54), NI(0.2),
    SO4(0.22), NI(0.2), SO4(0.22), NI(0.2), FA4(0.22), NI(1),
]
melody_2 = [
    DO(0.25), NI(0.05), RE(0.25), NI(0.05), MI(0.25), NI(0.05), DO(0.25), NI(0.05), NI(0.01), DO(0.25), NI(0.05), RE(0.25), NI(0.05),
    MI(0.25), NI(0.05), DO(0.25), NI(0.05), MI(0.25), NI(0.05), FA(0.25), NI(0.05), SO(0.5), NI(0.05),
    
    MI(0.25), NI(0.05), FA(0.25), NI(0.05), SO(0.5), NI(0.05), NI(0.01),
    SO(0.125), NI(0.05), LA(0.125), NI(0.05), SO(0.125), NI(0.05), FA(0.125), NI(0.05), MI(0.25), NI(0.05), DO(0.25), NI(0.05),
    
    SO(0.125), NI(0.05), LA(0.125), NI(0.05), SO(0.125), NI(0.05), FA(0.125), NI(0.05),
    MI(0.25), NI(0.05), DO(0.25), NI(0.05), RE(0.25), NI(0.05), SO(0.25), NI(0.05),
    DO(0.5), NI(0.05), NI(0.01),RE(0.25), NI(0.05), SO(0.25), NI(0.05), DO(0.5)
]
current_melody = melody_1

while True:
    # Lecture du potentiomètre en temps réel
    pot_value = potentiometer_pin.read_u16()
    vol = int((pot_value / 65535) * 1023)
    
    # Joue la note avec le volume adapté
    note, duration = current_melody[0]
    play_note(note, duration, vol)
    
    # Change la note
    current_melody.append(current_melody.pop(0))
```
## Tests du programme
https://github.com/hepl-decraye/smartcities/assets/159047970/b0dfa501-a664-4baf-9654-f41c29e8b4b2




