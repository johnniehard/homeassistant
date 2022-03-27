
## Förberedelser
### Ladda ned OS
Raspberry Pi OS (64-bit) Lite
https://www.raspberrypi.com/software/operating-systems/

### Flasha till minneskortet
Balena Etcher
https://www.balena.io/etcher/

### Tillåt SSH
I roten på "boot", skapa en tom fil som heter `ssh`.
I samma mapp, skapa följande fil för att ge tillgång till trådlöst nätverk.

wpa_supplicant.conf:
```
country=SE # replace with your country code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev

network={
	ssid="WIFI_NETWORK_NAME"
	psk="WIFI_PASSWORD"
	key_mgmt=WPA-PSK
}
```

### Hitta raspberryn på nätverket
Förberedelser nästan klara. Du kan nu ta ut minneskortet ur datorn, sätta in det i Raspberryn och koppla på elen.

Det sista vi behöver göra är att hitta raspberryn på nätverket.

`sudo nmap -sP 192.168.1.0/24`
(anpassa efter hur ditt nätverk ser ut)

SSH:a in med `pi@<IP-adresss>`, default lösenord: `raspberry`

## Setup
Installera ansible

```
sudo apt install ansible
```

Kopiera över ssh nyckel med `ssh-copyid`:
```
ssh-copy-id -i ~/.ssh/id_rsa.pub pi@<IP-adress>
```

Kör `setup.sh`
```
./setup.sh
```

## Starta upp

Kör playbook som kopierar över `docker-compose.yml` och kör docker-compose.

```
ansible-playbook -i inventory playbooks/homeassistant.yml
```

När den är klar så ska du kunna köra homeassistant onboardingen: http://[IP-adress]:8123

## TODO

* Syncthing - synk/backup av config mappen
* Tailscale - sätt upp tailscale för att kunna nå utanför lokala nätverket