# EXERCICE 3 : SYSTÈME DE CONTRÔLE DE TEMPÉRATURE 
## Objectif
L'objectif de ce projet est de développer un script MicroPython qui permettra de contrôler une LED RGB connectée à un Raspberry Pi Pico W, changeant sa couleur au rythme de la musique.
## Matériel
1. Raspberry Pi Pico W
2. Module microphone
3. Module LED RGB
4. Fils de connexion
## Consignes
• Initialisez le microphone et la LED RGB dans le script MicroPython.\
• Mettez en place une boucle qui lit constamment les données sonores du microphone.\
• Analysez les données sonores pour détecter les battements de la musique. Vous pouvez utiliser 
une méthode simple de détection des pics sonores ou implémenter un algorithme plus 
complexe pour une précision accrue.\
• Changez la couleur de la LED RGB de manière aléatoire à chaque fois qu'un battement est 
détecté. Assurez-vous que les changements de couleur sont visibles et synchronisés avec la 
musique.
## Bonus
• Calculez le rythme de la musique en BPM. Cela peut être réalisé en mesurant le temps entre 
les battements détectés.\
• À chaque minute, calculez la moyenne des BPM détectés pendant cette période et écrivez 
cette valeur dans un fichier texte sur le Raspberry Pi Pico W. Assurez-vous que le fichier est 
correctement ouvert, écrit, puis fermé pour éviter toute perte de données.
## Introduction
<p align = justify>
Ce rapport présente la réalisation d'un programme en MicroPython sur le Raspberry Pi Pico, visant à mettre en place un système de contrôle de température. Pour ce faire, nous avons utilisé divers composants tels que le capteur de température/humidité, la LED, le potentiomètre, l'écran LCD, et le Buzzer. L'objectif principal est de lire la valeur du potentiomètre, convertir cette valeur en une température de consigne, surveiller la température ambiante en temps réel, et afficher ces données sur l'écran LCD. Le programme inclut des fonctionnalités de contrôle, telles que l'activation de la LED et l'émission d'une alarme en cas de dépassement de la température de consigne. Des fonctionnalités bonus, comme le battement progressif de la LED et le clignotement/défilement de l'alarme à l'écran on été ajouté. 
</p>

## Câblage
Pour ce projet plusieurs modules ont été intégré au rapsberry pico : potentiomètre, led, buzzer, capteur DHT11 et l'écran lcd pour l'affichage (voir figure).
<p align = center>
<img src="">
</p>

## Explication du code
