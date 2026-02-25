# Cisco Network Management: NTP & Syslog Configuration
Ce projet démontre la mise en place d'une infrastructure réseau de base sous Cisco Packet Tracer, axée sur la synchronisation du temps (NTP) et la centralisation des journaux d'événements (Syslog).

🌐 Topologie du Réseau
![Topologie du réseau](./syslog%20ntp%20server.PNG)

L'architecture utilise un serveur central comme source de temps et destination des logs.

1 Routeur Cisco 2911 (Passerelle : 192.168.1.1)

1 Switch Cisco 2960 (Gestion : 192.168.1.5)

1 Serveur Générique (Services : 192.168.1.10)

⚙️ Configuration des Équipements
# 1. Routeur R1 (192.168.1.1)
<pre>
enable
configure terminal
hostname R1

! --- Configuration Interface ---
interface GigabitEthernet 0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit

! --- Temps et Logs ---
ntp server 192.168.1.10
service timestamps log datetime msec
logging host 192.168.1.10
logging on
</pre>
# 2. Switch S1 (192.168.1.5)
<pre>
enable
configure terminal
hostname S1

! --- Interface de Gestion ---
interface vlan 1
 ip address 192.168.1.5 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.1.1

! --- Temps et Logs ---
ntp server 192.168.1.10
service timestamps log datetime msec
logging host 192.168.1.10
 
</pre>
# 3. Serveur (192.168.1.10)
<pre>
 
Service NTP : Activé (Source de temps).

Service Syslog : Activé (Réception des messages).
 
</pre>
✅ Vérification
Pour valider la configuration, les commandes suivantes ont été utilisées :

- show ntp associations : Confirme que l'astérisque (*) est présent devant l'IP du serveur.

- show clock : Vérifie que l'heure est synchronisée sur la bonne date.

- show logging : Vérifie que l'envoi vers l'hôte 192.168.1.10 est actif.

📖 Règle d'apprentissage

Ce dépôt sert de destination pour documenter les compétences acquises sur la gestion des services réseau Cisco.
