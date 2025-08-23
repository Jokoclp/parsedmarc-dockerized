# parsedmarc-dockerized

old Version für Rollback:
elasticsearch:8.13.2 & kibana:8.13.2


currently 8.17.0 & 8.17.0

## Setup:
PARSEDMARC 8.17 install guide - ubuntu 22.04 / Abweichend bei anderen Distributionen

root user
```sudo bash```

Download und Installation docker & docker compose-plugin
```
curl -sSL https://get.docker.com/ | CHANNEL=stable sh
systemctl enable --now docker
apt install docker-compose-plugin
```

Download der Repository für  elasticsearch:8.13.2 & kibana:8.13.2
```
git clone https://github.com/Jokoclp/parsedmarc-dockerized.git /opt/parsedmarc-dockerized/
```

Sample ini zu ini
```
cd /opt/parsedmarc-dockerized/ && cp data/conf/parsedmarc/config.sample.ini data/conf/parsedmarc/config.ini
```

config editieren
```
nano /opt/parsedmarc-dockerized/data/conf/parsedmarc/config.ini
```


GEOIPUPDATE ja / nein? (sorgt für die korrekte Anzeige der Karte -> IP zu Geolocation)
wenn ja ->  
```
nano data/conf/geoipupdate/geoipupdate.env
```

installation von apache utilitys & nginx proxy benutzer anlegen (USERNAME ändern)
```
apt install apache2-utils
htpasswd -c data/conf/nginx/htpasswd USERNAME
```


Container starten
```
cd /opt/parsedmarc-dockerized/
docker compose up -d
```

Warten bis alle Container Healthy & Running sind (kann ein bisschen dauern)
https://HOSTIP/
mit htpasswd User und dazugehörigem Passwort anmelden.






## Credits

Built with [parsedmarc](https://github.com/domainaware/checkdmarc), [Elasticsearch and Kibana](https://www.elastic.co/), [nginx](https://nginx.org), [Docker](https://docker.com) and [MaxMind GeoIP](https://dev.maxmind.com/geoip/geoip2/geolite2/). 

Based on [patschi's docker container](https://github.com/patschi/parsedmarc-dockerized) & [accolons](https://github.com/accolon/parsedmarc-dockerized) work.










