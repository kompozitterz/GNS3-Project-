# SOC LAB GNS3 #

## 1. Configuration matérielle ##
    * OS: Mac puce Sillicon  
    * RAM: 16 Go
    * Hyperviseur: UTM  

## 1. Introduction ##

Ce projet consitste sécuriser une infrastucture réseau domestique 
et professionnel. Il utilise le logiciel GNS3 ainsi qu'une machine
virtuelle Ubuntu Server(UMT) sur laqulle GNS3 tourne, ce qui fait 
de la VM un gateway. Dans cette machine, nous allons faire tourner 
d'autres VM's (Parrot, Rocky et Alpine Linux, les appliances GNS3, 
etc..) pour des raisons :
### * de compatibilité: 
La plupart des appliances sont compatibles avec des architectures
amd64 et non arm64, ce qui est mon cas.
### * de performances:
Faire tourner des VM's en vitualisation ou émulation avec des outils de monitoring en temps réel tels que Wazuh ou Splunk consomment beacoup
de mémoire, ce qui n'est pas adapté à notre configuration.

Pour lancer le projet, veuillez lire le fichier GETTING_STARTED.md.
