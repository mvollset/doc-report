---
layout: posts
---
# Rapport

## Hva en rapport består av.

En rapport består av minimum to filer en json fil som beskriver rapporten og en sql fil med spørringen til rapporten. En rapport bør også inneholde en README.md som er dokumentasjonen på rapporten. En kan også legge inn en markdown fil med tekst som legges inn på forsiden rapportlista. Det er også mulig å inkludere en js fil i en rapport. Det må spesifiseres i rapportopsjonene hvilken fil dette er. 

```
myreport/
  |- report.json
  |- report.sql
  |- README.md
  |- blurb.md
  |- help.md
  +- repconfig.js
```
## Filenes innhold og rolle.

### report.json
Denne definerer selve rapporten og rapportens egenskaper og attributter.

### report.sql
Inneholder selve spørringen til rapporten.

### README.md 
Utviklers dokumentasjon. Gjerne teknisk dokumentasjon.

### blurb.md
Kort beskrivelse til evt. forside.

### help.md
Lengre beskrivelse som vises øverst på rapport siden.

### repconfig.js
Javascript fil som inneholder evt ekstra kode som følger med rapporten. Dette kan være aggregatorer, formatterere etc.


### Rapport opsjoner.
Følgende egenskaper kan settes på en rapport:
 - **name** Dette er påkrevd og må kunne brukes i en url
 - **title** Tittel, navnet som brukeren forholder seg til
 - **sqlfile** Navn på fil med spørring.
 - [show] true/false default false, skal rapporten vises på forsiden.
 - [view] Viewet som rapporten skal bruke
 - [blurb] Navn på markdown fil som inneholder en beskrivelse av rapporten
 - [img] URL til bilde som skal vises på forsiden sammen med rapporten.
 - [parameters] Array av rapport parametere.
 - [runonget] true/false default true Skal rapporten kjøre automatisk hvis alle påkrevde parametere er utfylt.
 - [configfile] peker til javascript fil, uten .js , som vil ble lastet og sendt ut i viewet når rapporten kjører.
 - [tags] Array av tags, brukes til å styre informasjonen på første siden.
 - [helpfile] Lengre beskrivelse som vises på selve rapportsiden.



