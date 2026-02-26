# TP 02 : Mise en œuvre des ACL Standards et Étendues

## 📋 Description
Ce laboratoire pratique sur Cisco Packet Tracer vise à sécuriser les flux réseau entre un segment LAN (utilisateurs) et un serveur distant. L'objectif est de maîtriser la différence entre le filtrage par adresse IP source (Standard) et le filtrage granulaire par protocole/port (Étendue).

## 🏗️ Topologie
- **Réseau Local (LAN) :** `192.168.1.0/24`
- **Passerelle (Router0) :** `192.168.1.254`
- **Serveur (Server0) :** `1.1.1.2/24`

## 🛡️ Stratégie de Sécurité (Règle d'or)
Pour optimiser les performances et la précision, j'applique la règle suivante :

1. **ACL Étendue :** Placée au plus proche de la **Source**. On bloque le trafic inutile dès l'entrée du routeur (`Inbound`) pour économiser les ressources CPU.
2. **ACL Standard :** Placée au plus proche de la **Destination**. Comme elle ne filtre que par IP source, on l'applique en sortie (`Outbound`) sur l'interface finale pour ne pas impacter les autres destinations possibles.

---

## ⚙️ Configuration des ACL

### 1. ACL Standard (Interdire PC0 vers Serveur)
Placée sur l'interface `g0/0/1` en direction du serveur.
<pre>
access-list 10 deny host 192.168.1.1
access-list 10 permit any
interface GigabitEthernet 0/0/1
ip access-group 10 out
</pre>

### 2. ACL Étendue (Filtrage granulaire pour PC1)
Autorise le Web (HTTP), mais bloque le PING (ICMP) vers le serveur. Placée sur g0/0/0 en entrée.
<pre> 
access-list 100 permit tcp host 192.168.1.2 host 1.1.1.2 eq 80
access-list 100 deny icmp host 192.168.1.2 host 1.1.1.2
access-list 100 permit ip any any
interface GigabitEthernet 0/0/0
ip access-group 100 in
</pre>

<pre>
Source,Destination,Protocole,Résultat Attendu
PC0,Serveur,Tout,❌ Bloqué (ACL 10)
PC1,Serveur,HTTP (Port 80),✅ Autorisé (ACL 100)
PC1,Serveur,ICMP (Ping),❌ Bloqué (ACL 100)
PC2,Serveur,Tout,✅ Autorisé (Permit Any)
</pre>

# Commandes de diagnostic :
<pre>
show ip access-lists : Pour vérifier les compteurs de paquets (matches).

show ip interface g0/0/x : Pour confirmer la direction du filtrage.
</pre>
