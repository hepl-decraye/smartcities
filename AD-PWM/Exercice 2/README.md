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




