# Travail demandé
Cet exercice consiste à créer un circuit avec une LED et un bouton. Lors du premier appui, la LED commence à clignoter, le deuxième appui accélère la fréquence de clignotement, et le troisième éteint la LED. Ce cycle se répète à chaque nouvelle pression sur le bouton.
## Introduction
Dans un premier temps, j'ai choisi de gérer l'appui sur le bouton-poussoir via une interruption. Cette approche permet une mise à jour instantanée à chaque pression sur le bouton, assurant ainsi une réactivité optimale dans la détection des nouveaux appuis.
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
## Explication du code
1. Les bibliothèques machine et utime sont importées.
2. Une broche (pin) est configurée comme entrée (IN) et connectée à un bouton physique sur la carte (pin 16). Une autre broche est configurée en tant que sortie (OUT) et connectée à une LED (pin 18).
3. Une variable globale val est déclarée et initialisée à 0. Cette variable sera utilisée pour compter le nombre de pressions du bouton.
4. Une fonction bouton_callback(pin) est définie. Cette fonction est appelée lorsqu'un événement de chute de tension (IRQ_FALLING) est détecté sur la broche du bouton. À chaque appel de cette fonction, la variable globale val est incrémentée de 1.
5. L'interruption (IRQ) sur la broche du bouton est configurée pour déclencher la fonction bouton_callback lorsqu'une chute de tension est détectée.
6. Une boucle infinie while True est utilisée pour surveiller la valeur de la variable val. Selon la valeur de val, différentes actions sont effectuées :
/
Si val est égal à 1, la LED est basculée (allumée ou éteinte) et le programme attend 0.3 seconde avant de passer à l'itération suivante de la boucle.
/
Si val est égal à 2, la LED est à nouveau basculée, mais cette fois avec un délai de 0.1 seconde.
/
Si val est égal à 3, la LED est éteinte, la variable val est réinitialisée à 0, et le programme attend 1 seconde avant de passer à l'itération suivante.
## Le GIF
![](https://github.com/hepl-decraye/smartcities/blob/main/gifled.gif)
