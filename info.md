Om jouw Raspberry Pi Zero 2 W via Tailscale toegang te geven tot een gedeelde datamap op je ZimaOS-server, moet je de map via een netwerkprotocol (zoals SMB of NFS) delen over het beveiligde Tailscale-IP-adres van je ZimaOS.
## Stappenplan## 1. ZimaOS-server instellen via Tailscale

   1. Zorg dat de Tailscale-app is geïnstalleerd op je ZimaOS via de ZimaOS App Store en dat deze is gekoppeld aan hetzelfde Tailscale-account.
   2. Zoek het Tailscale IP-adres (begint vaak met 100.x.y.z) van je ZimaOS-server op via het Tailscale Dashboard.
   3. Deel de gewenste datamap in de ZimaOS-instellingen (meestal standaard actief via SMB/Samba voor gedeelde mappen).

## 2. Netwerkmap koppelen op de Raspberry Pi Zero 2 W
Open de terminal van je Raspberry Pi en voer de volgende stappen uit:

   1. Installeer de benodigde software om SMB-mappen te kunnen lezen:
   
   sudo apt update && sudo apt install cifs-utils -y
   
   2. Maak een lokale map aan op de Pi waar de ZimaOS-map in moet verschijnen (het koppelpunt):
   
   mkdir ~/zima_data
   
   3. Koppel de map handmatig om te testen (vervang 100.x.y.z door het Tailscale IP van je ZimaOS en mapnaam door de naam van de gedeelde ZimaOS-map):
   
   sudo mount -t cifs -o username=JOUW_ZIMA_GEBRUIKERSNAAM //100.x.y.z/mapnaam ~/zima_data
   
   Er zal nu om je ZimaOS-wachtwoord worden gevraagd.

## 3. Automatisch koppelen bij het opstarten (Optioneel)
Als je wilt dat de Pi de map altijd automatisch koppelt zodra Tailscale actief is, voeg je deze toe aan het /etc/fstab-bestand:

   1. Open het bestand:
   
   sudo nano /etc/fstab
   
   2. Voeg onderaan de volgende regel toe (alles op één regel):
   
   //100.x.y.z/mapnaam /home/pi/zima_data cifs username=JOUW_GEBRUIKERSNAAM,password=JOUW_WACHTWOORD,x-systemd.automount,x-systemd.requires=tailscaled.service 0 0
   
   (Let op: x-systemd.requires=tailscaled.service zorgt ervoor dat de Pi wacht tot Tailscale volledig is opgestart voordat hij verbinding probeert te maken).
   3. Sla op met CTRL+O, druk op Enter, en sluit af met CTRL+X.

Wil je de map delen via SMB (Samba) of geef je de voorkeur aan NFS voor betere prestaties op de Pi Zero?

