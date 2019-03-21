Registry compose
================

Dit is een compose file om een docker registry te maken.

htpasswd
--------

Toegang tot de registry gaat met een wachtwoord. Deze moet je aanmaken en opslaan als secret alvorens je de registry kan starten. Aanmaken van een wachtwoord gaat met het 'htpasswd' commando uit het image:
- docker run --entrypoint htpasswd registry:0.1 -Bbn [USER] [PASSWORD] > htpasswd
- docker secret create registry_htpasswd htpasswd

certificaat
-----------
De registry heeft een certificaat nodig om met https bereikbaar te zijn. Het aanmaken van een certificaat gaat met:
- openssl genrsa -out server.key 2048
- openssl req -new -key server.key -out server.csr
- openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt

Je kunt het certificaat lezen met:
- openssl req -text -noout -in server.csr


En vervolgens als secret opslaan:
- docker secret create registry_cert server.crt 
- docker secret create registry_cert_key server.key
