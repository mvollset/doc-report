---
layout: default
---
# Rapportmotor (Stupid reporting)

Målet med SRE er å lage et enkelt og fleksibelt rammeverk som kan være utgangspunktet for ad hoc rapporter som vi leverer i kundeprosjekter. Rapporter skal kunne leveres som tekstfiler og enkelt kunne gjenbrukes i andre prosjekter. Selve SRE er laget i node.js slik at også serveren er "tekst" filer, dette gjør det mulig å være cowboy også.

## Databasestøtte 
Foreløpig støtter SRE:
  - MS SQL
  - Oracle
  - Postgresql
>Postgresql har foreløpig ikke støtte for "named parameters". For oraclestøtte kreves det, foreløpig, Visual Studio og Oracle klient installert.

## Hvordan komme i gang.
- Installer [node](https://nodejs.org/en/) 
- Installer en git klient, f.eks [github for windows](https://desktop.github.com/)
- Installer grunt client, kjør "npm install -g grunt-cli" fra kommandolinja.
- Hent kildekoden fra bitbucket https://mvollset@bitbucket.org/syscom-rapport/report-engine.git (Hvis du ønsker tilgang er det bare å spørre meg.)
- Installer en god teksteditor, evt bruk Visual Studio med nodejs plugin, den ser lovende ut, men er foreløpig veldig langsom.

Se forøvrig [Getting Stupid](getting-stupid.html) for en steg for steg forkalring av hvordan komme i gang.


### Version
0.9.1

### Tekniske greier

Report engine bruker mange open source komponener:
* [node.js](http://nodejs.org) - evented I/O for the backend
* [Express](http://expressjs.com) fast node.js network app framework
* [Jade](http://jade-lang.com) Templating system
* [Grunt](http://gruntjs.com) - streaming build system
* [Foundation](http://foundation.zurb.com) - Responsive front end
* [jQuery](http://jquery.com) - duh

Og et som ikke er opensource, men er viktig for det:
* [Highcharts](http://highcharts.com) For visualisering.








