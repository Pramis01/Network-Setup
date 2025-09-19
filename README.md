Nettverk og tilkobling – Dokumentasjon

Tilkobling

-Både Raspberry Pi (Ubuntu OS, server) og laptop (Windows, klient) ble koblet til klassens LAN:

	-SSID: 2IMA

	-Passord: IMKuben1337!


Oppsett

-Server (Raspberry Pi / Ubuntu) fikk en statisk IP-adresse satt opp manuelt.

-Subnet mask ble satt til 255.0.0.0 (/8) slik at Pi og klient-PCen kunne kommunisere.

-Klienten (Windows PC) beholdt eksisterende IP (fikk den via DHCP).

IP-adresser i oppsettet

Enhet		Rolle	IP-adresse	Subnet mask
Raspberry Pi	Server	10.200.10.26	255.0.0.0	
Windows PC	Klient	10.2.2.85	255.0.0.0	

Forsøk med avansert metode (Netplan)

Jeg forsøkte først å sette statisk IP via Netplan i terminalen med:

 	- sudo nano /etc/netplan/01-netcfg.yaml


Jeg skrev inn følgende konfigurasjon (ca. 18 linjer langt):

		network:
		  version: 2
		  renderer: NetworkManager
		  wifis:
		    wlan0:
		      dhcp4: no
		      addresses:
		        - 10.200.10.26/8
		      nameservers:
		        addresses:
		          - 8.8.8.8
		          - 1.1.1.1
		      routes:
		        - to: 0.0.0.0/0
		          via: 10.200.10.1 
		      access-points:
		        "2IMA":
		          password: "IMKuben1337!"


- Problemet: YAML er veldig følsomt for innrykk og mellomrom.Jeg lærte hvordan oppsettet fungerer, men fikk det ikke til å virke i praksis.

Endelig løsning (GUI – enkel måte)

Jeg satte IP-adressen via nettverksinnstillinger i GUI:

1.Åpnet WiFi-innstillinger

2.Valgte nettverket 2IMA

3.Gikk til IPv4

4.Endret metode til Manuell

5.Satt inn:

	Adresse: 10.200.10.26

	Nettmaske: 255.0.0.0

Etterpå koblet jeg til på nytt, og Pi fikk statisk IP-adresse.

<img width="2560" height="1440" alt="GUI_Statisk_IP" src="https://github.com/user-attachments/assets/b4a56328-9f07-4415-aa4f-f1ec82a41caa" />

Test

- Ping fra klient → server: 

		ping 10.200.10.26

<img width="2560" height="1440" alt="PING_WIN11 (1)" src="https://github.com/user-attachments/assets/b0ab7d2c-1125-4f3c-b47f-e23b9da118cb" />

-Ping fra server → klient: ✅

		ping 10.2.2.85
 
<img width="1115" height="628" alt="Skjermbilde 2025-09-19 094517" src="https://github.com/user-attachments/assets/98c154fa-aa3c-4e1c-8208-1ad0f06ad04b" />

 


Konklusjon

-Begge maskiner var koblet til samme LAN (2IMA).

-Raspberry Pi fikk statisk IP via GUI.

-Ping mellom klient og server fungerte.

-Jeg prøvde Netplan først, men valgte GUI som en enklere løsning.

-Dette ga meg både forståelse for den avanserte metoden og et fungerende oppsett.

