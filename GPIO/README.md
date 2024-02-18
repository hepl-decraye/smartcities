# EXERCICE 1 : CLIGNOTEMENT DE LED AVEC BOUTON POUSSOIR 
## Objectif
Créer un programme MicroPython qui permet de faire clignoter une LED à différentes vitesses en 
fonction du nombre de pressions sur un bouton poussoir.
## Matériel
• Microcontrôleur compatible MicroPython (Raspberry Pi Pico)\
• Module LED\
• Module bouton poussoir\
• Câbles
## Consignes
1. Branchez la LED et le bouton poussoir au microcontrôleur 
2. Ecrivez un programme MicroPython qui répond aux exigences suivantes : \
o La LED doit clignoter à l’infini avec une fréquence de 0,5 Hz lorsque le bouton poussoir 
est pressé une fois.\
o La LED doit clignoter plus vite lorsque le bouton poussoir est pressé une second fois.\
o La LED doit s'éteindre lorsque le bouton poussoir est pressé une troisième fois.
3. Testez votre programme et vérifiez qu'il fonctionne correctement.
## Bonus
• Ajoutez un délai ou un effet dans les clignotements de la LED ou dans le passage d’une vitesse 
de clignotement à une autre.\
• Modifiez le nombre d'appuis nécessaires pour changer la vitesse de clignotement.
## Introduction
Dans un premier temps, j'ai choisi de gérer l'appui sur le bouton-poussoir via une interruption. Cette approche permet une mise à jour instantanée à chaque pression sur le bouton, assurant ainsi une réactivité optimale dans la détection des nouveaux appuis.
## Cablage
<p align="center">
<img src="https://github.com/hepl-decraye/smartcities/blob/main/images/gpio.png">
</p>

## Le code

```ruby
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
        utime.sleep(0.5)
    elif val == 2:
        led.toggle()
        utime.sleep(0.2)
    elif val == 3:
        led.value(0)
        val = 0
        utime.sleep(1)
```
## Explication du code
1. Les bibliothèques machine et utime sont importées.
2. Une broche (pin) est configurée comme entrée (IN) et connectée à un bouton physique sur la carte (pin 16). Une autre broche est configurée en tant que sortie (OUT) et connectée à une LED (pin 18).
3. Une variable globale val est déclarée et initialisée à 0. Cette variable sera utilisée pour compter le nombre de pressions du bouton.
4. Une fonction bouton_callback(pin) est définie. Cette fonction est appelée lorsqu'un front descendant (IRQ_FALLING) est détecté sur la broche du bouton. À chaque appel de cette fonction, la variable globale val est incrémentée de 1.
5. L'interruption (IRQ) sur la broche du bouton est configurée pour déclencher la fonction bouton_callback lorsqu'un front descendant est détecté.
6. Une boucle infinie while True est utilisée pour surveiller la valeur de la variable val. Selon cette variable, différentes actions sont effectuées :

    * Si val est égal à 1, la LED est basculée (allumée ou éteinte) et le programme attend 0.3 seconde avant de passer à l'itération suivante de la boucle.

    * Si val est égal à 2, la LED est à nouveau basculée, mais cette fois avec un délai de 0.1 seconde.

    * Si val est égal à 3, la LED est éteinte, la variable val est réinitialisée à 0.
## Test du programme
![](https://github.com/hepl-decraye/smartcities/blob/main/gifled.gif)
## Conclusion
<p align="justify">
En conclusion, cet exercice a abouti à la création d'un programme efficace en MicroPython pour contrôler le clignotement d'une LED en fonction du nombre de pressions sur un bouton poussoir. L'utilisation d'une interruption assure une réactivité optimale, et le code élaboré a été expliqué de manière détaillée, avec des tests pour confirmer son bon fonctionnement.
</p>

