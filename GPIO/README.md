# Travail demandé
Cet exercice consiste à créer un circuit avec une LED et un bouton. Lors du premier appui, la LED commence à clignoter, le deuxième appui accélère la fréquence de clignotement, et le troisième éteint la LED. Ce cycle se répète à chaque nouvelle pression sur le bouton.
## Introduction
Pour commencé j'ai décicé de gérer l'appui sur le bouton poussoir par interruption afin de pouvoir au mieux actualiser le nouvel appui sur le bouton.
## Le code
Voici un lien vers le code complet [code](exercice-1)
```
import machine
import utime

bouton = machine.Pin(16, machine.Pin.IN)
led = machine.Pin(18, machine.Pin.OUT)

val = 0

def bouton_callback(pin):
    global val
    val += 1

bouton.irq(trigger=machine.Pin.IRQ_FALLING, handler=bouton_callback)

while True:
    if val == 1:
        led.toggle()
        utime.sleep(0.3)
    elif val == 2:
        led.toggle()
        utime.sleep(0.1)
    elif val == 3:
        led.value(0)
        val = 0
        utime.sleep(1)
```
