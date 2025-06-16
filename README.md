# BaumWebTest

#anmelden
User:debian
Password:debian

#su für layout
sudo su -
loadkeys de

#Update, wenn fehler, dann Probleme mit Netzwerk
sudo apt-get update && apt-get upgrade


#Root bleiben, SSH installieren
apt-get install tree #sinnvoll zu haben
apt-get install ssh
apt-get install apache2

#IP-Adresse einsehen (Wahrscheinlich NAT-Adresse in VM, wenn nicht anders konfiguriert)
#Geht nur in eine Richtung, wenn kein Port-Forwarding
ip a

#In Virtualbox Einstellungen auf Netzwerkbrücke umstellen, damit die VM gleichberechtigt im Netz angezeigt wird
#WLAN verwendet eine andere Brücke

#Netzwerkkarte aus und wieder anschalten, sollte nach Umstellung auf Netzwerk-Bridge IP im lokalen Netz bekommen
ifdown enp0s3
ifup enp0s3

#In der Windows Ps3 sollte man jetzt eine SSH-Session starten, username@ip-address
ssh [username]@[ip-address]

#Anzeigen der Sessions
who

#IP-Adresse anpassen
nano /etc/network/interfaces

########################
alt
########################
allow-hotplug enp0s3
iface enp0s3 inet dhcp
########################
neu
########################
allow-hotplug enp0s3
iface enp0s3 inet static
address 10.204.120.111/24 
#netmask 255.255.255.0 # oder CIDR in der Zeile drüber
gateway 10.204.120.254 # ggf. auf Win Host mit "ipconfig all" nachschauen
dns-servers 10.204.110.1 # kann wieder mit ipconfig nachgeschaut werden
########################

#Speichern und Editor verlassen
Strg+O
Strg+X

#Schnittstelle neustarten
ifdown enp0s3
ifup enp0s3

#überprüfen
ip a 

#neues Passwort vergeben
passwd debian

#Status von webserver einsehen
systemctl status apache2

#default page kann in einem Browser angesehen werden
#Speicherort der default page: /var/www/html/index.html

#Eigene Seite mit Benutzer anlegen
cd /var/www
mkdir tux

tree
ls -l

#Benutzer anlegen
adduser tux
####
password: tux
vollständiger Name: tux
skip rest…
####

#F2 um Konsole zu wechseln und als tux anmelden

#Rechte zuweisen, mit  "-r" wenn man schon Inhalt drinnen hat (recursive)
chown tux:tux ./tux

#
cd /etc/apache2
ls

#nach sites-available schauen
cd /sites-available
ls

# 0000-default.conf kopieren
cp 0000-default.conf tux.conf

#conf anpassen
nano ./tux.conf

#####
Servernamen und Dokumenten-Root-Pfad anpassen
#####
Servername tux.org

[…]

DocumentRoot var/www/html/tux
#####

#alte Seite deaktivieren und neue aktivieren
a2ensite tux.conf # apache2 enable site
a2dissite tux.conf # apache 2 disable site

#Änderungen übernehmen
systemctl reload apache2

#WinSCP installieren 
# ip von Server angeben
# port 22
# Nutzer: tux
# Passwd: tux

#HTML von Scratch besorgen
# Adresse: \\10.204.110.2\scratch\Br\Inhalt\HTML\designs
# Zip auf eigenen Rechner ablegen, entpacken und dann auf Web-Server kopieren
![grafik](https://github.com/user-attachments/assets/9c18918b-4b33-4343-a0c4-207f7080f4cd)




Host file für DNS  unter Windows: 
C:\Windows\System32\drivers\etc![grafik](https://github.com/user-attachments/assets/33e1de2a-87da-4fc4-970a-d31f51c4bc7e)

