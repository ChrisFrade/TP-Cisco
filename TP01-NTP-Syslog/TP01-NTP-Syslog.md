# Cisco Network Management: NTP & Syslog Configuration
Ce projet démontre la mise en place d'une infrastructure réseau de base sous Cisco Packet Tracer, axée sur la synchronisation du temps (NTP) et la centralisation des journaux d'événements (Syslog).

🌐 Topologie du Réseau
![Topologie du réseau](./syslog%20ntp%20server.PNG)

L'architecture se compose de :

1 Routeur Cisco 2911 (Passerelle : 192.168.1.1)

1 Switch Cisco 2960 (Gestion : 192.168.1.5)

1 Serveur Générique (Services : 192.168.1.10)

⚙️ Configuration des Équipements
# 1. Serveur (Services Destination)
<pre>
 IP : 192.168.1.10 /24

Services activés : * NTP : Sert de source de temps pour le réseau.

Syslog : Reçoit et stocke les logs du routeur et du switch.
</pre>
# 2. Routeur R1 (Configuration IP & Logs)
<pre>
! --- Configuration du Routeur R1 ---
enable
configure terminal
hostname R1

! Configuration NTP
ntp server 192.168.1.10
service timestamps log datetime msec

! Configuration Syslog
logging host 192.168.1.10
logging on
 
</pre>
# Adressage IP
<pre>
 
 interface g0/0

 ip address 192.168.1.1 255.255.255.0
 
 no shutdown
 
exit
</pre>
# Temps & Logs
<pre>
 ntp server 192.168.1.10

service timestamps log datetime msec

logging host 192.168.1.10

logging on
</pre>
# 3. Switch S1 (Configuration IP & Logs)
<pre>
 enable

conf t

hostname S1
</pre>
# Adressage IP (VLAN Management)
<pre>
 interface vlan 1

 ip address 192.168.1.5 255.255.255.0
 
 no shutdown
 
exit

ip default-gateway 192.168.1.1
</pre>
# Temps & Logs
<pre>
 ntp server 192.168.1.10

service timestamps log datetime msec

logging host 192.168.1.10
</pre>
✅ Vérification
Pour valider la configuration, les commandes suivantes ont été utilisées :

- show ntp associations : Confirme que l'astérisque (*) est présent devant l'IP du serveur.

- show clock : Vérifie que l'heure est synchronisée sur la bonne date.

- show logging : Vérifie que l'envoi vers l'hôte 192.168.1.10 est actif.

📖 Règle d'apprentissage
Source pour apprendre, Destination pour transmettre.
Ce dépôt sert de destination pour documenter les compétences acquises sur la gestion des services réseau Cisco.
