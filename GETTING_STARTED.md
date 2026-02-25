Ce guide explique comment initialiser et lancer l'infrastructure réseau segmentée sous GNS3 avec Docker.
## Prérequis

    GNS3 Client (v2.2.56.1 ou supérieure).

    GNS3 VM installée et configurée.

    Les images Docker suivantes téléchargées dans GNS3 :

        alpine:latest (Routeur).

        parrotsec/core (Attaquant).

        rockylinux:9.3 (Cible).

        openvswitch/ovs (Switches SNMP).

## Procédure de Lancement
### 1. Nettoyage des processus fantômes

Avant de lancer GNS3, il est impératif de s'assurer qu'aucun conteneur ne crée de conflit de nom (Erreur 409).

    Connectez-vous à la console de la GNS3 VM.

    Supprimer les conteneurs persistants qui bloquent les ports TCP (5010, 5011) :

    ```sh
    docker rm -f $(docker ps -a -q)
    ```

### 2. Ouverture du Projet

    Lancez le GNS3 Client : 

    ```sh
    source gns3-env/bin/activate
    gns3server --host 0.0.0.0 --port 3080
    ```

    Vérifiez la connexion au "Compute local" (127.0.0.1:3080) dans les logs.

    Ouvrez GNS3 (application) sur la machine hôte et le projet SOC LAB gns3.

### 3. Initialisation du Réseau

Une fois le schéma affiché, lancez les nœuds dans cet ordre :

    AlpineLinux-1 (Routeur) : Attend qu'il récupère son IP NAT (eth0).

    OpenvSwitch-1 & 2 : Les switches doivent monter leur interface br0. 

    Parrotsec & Rocky : Les terminaux finaux.

## Vérification de la Connectivité

Pour valider que l'installation est opérationnelle, effectuez les tests suivants :
Source	Destination	Commande	Résultat attendu
Parrot	Switch Attaque	ping 10.0.20.250	0% packet loss
Parrot	Passerelle Alpine	ping 10.0.20.254	Reply from gateway
Parrot	Cible Rocky	ping 10.0.10.20	Routage Inter-VLAN OK

## Dépannage Rapide

    Erreur 409 Conflict : Des conteneurs Docker portent déjà le nom de vos nœuds. Référez-vous à l'étape 1 du guide.

    Network is unreachable : Vérifiez que la route par défaut est configurée sur le terminal (ip route add default via 10.0.X.254).

    Timeout SNMP : Assurez-vous que l'IP de management du switch est bien sur l'interface br0 et non eth0.