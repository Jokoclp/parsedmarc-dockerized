# parsedmarc-dockerized

old Version für Rollback:
elasticsearch:8.13.2 & kibana:8.13.2


currently 8.17.0 & 8.17.0

## Setup:
## PARSEDMARC 8.13 install guide - ubuntu 22.04 / Abweichend bei anderen Distributionen

# root user
sudo bash

# Download und Installation docker & docker compose-plugin
curl -sSL https://get.docker.com/ | CHANNEL=stable sh
systemctl enable --now docker
apt install docker-compose-plugin


# Download der Repository für  elasticsearch:8.13.2 & kibana:8.13.2
git clone https://github.com/Jokoclp/parsedmarc-dockerized.git /opt/parsedmarc-dockerized/

# Sample ini zu ini
cd /opt/parsedmarc-dockerized/ && cp data/conf/parsedmarc/config.sample.ini data/conf/parsedmarc/config.ini

# config editieren
nano /opt/parsedmarc-dockerized/data/conf/parsedmarc/config.ini


# GEOIPUPDATE ja / nein? (sorgt für die korrekte Anzeige der Karte -> IP zu Geolocation)
# wenn ja ->  
nano data/conf/geoipupdate/geoipupdate.env

# installation von apache utilitys & nginx proxy benutzer anlegen (USERNAME ändern)
apt install apache2-utils
htpasswd -c data/conf/nginx/htpasswd USERNAME


# Container starten
cd /opt/parsedmarc-dockerized/
docker compose up -d

# Warten bis alle Container Healthy & Running sind (kann ein bisschen dauern)
# https://HOSTIP/
# mit htpasswd User und dazugehörigem Passwort anmelden.





### What's happening then?

1. First, containers of the stack are created and started. This might take a while, as several containers have dependencies on others being in a healthy state (meaning that its service must be fully started).
2. During the startup of the `parsedmarc-init` container, all required steps and preparations are being taken care of - like generating a self-signed certificate for the included `nginx` webserver.
3. Once the Kibana container - where you can view the dashboards - is started up, the corresponding parsedmarc dashboards are automatically imported into Kibana by the `parsedmarc-init` container.
4. After some while, when everything is up and running, you can then access Kibana and its dashboards under the shipped reverse proxy at `https://HOST_IP` (Make sure to use HTTPS!). There will be a warning due to the self-signed certificate. The default username/password for HTTP basic authentication is admin/admin. You should change this!

## Credits

Built with [parsedmarc](https://github.com/domainaware/checkdmarc), [Elasticsearch and Kibana](https://www.elastic.co/), [nginx](https://nginx.org), [Docker](https://docker.com) and [MaxMind GeoIP](https://dev.maxmind.com/geoip/geoip2/geolite2/). 

Based on [patschi's docker container](https://github.com/patschi/parsedmarc-dockerized) & [accolons](https://github.com/accolon/parsedmarc-dockerized) work.



