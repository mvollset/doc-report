---
layout: posts
---
# Dato makroer

I dato felt er det mulig å legge inn "makro" verdier. Dette gjør det mulig å lage default verdier på datoparametere som endrer seg over tid. Makroene er:

## idag,today - enhet dager
Gir dagens dato

## imorgen, tomorrow - enhet dager
Gir i morgendagens dato

## igår,igar,yesterday - enhet dager
Gir gårsdagens dato

## now,nå - enhet sekunder
Gir klokkeslettet nå

## ukestart,startofweek - enhet uker
Starten på denne uka.

## ukeslutt,endofweek - enhet uker
Slutten på uka.

## månedsstart,manedsstart,startofmonth - enhet måneder
Starten på nåværende måned

## månedsslutt,manedssslutt,endofmonth - enhet måneder
Slutten på nåværende måned

>Alle makroer kan angis med +/-N slik at det trekkes fra eller legges til N av enhet til datoen. F.eks 
 * idag-4 er for 4 dager siden. 
 * now-60 er for et minutt siden
 * månedsstart-1 er starten av forrige måned
 * ukeslutt-1 er slutten på forrige uke, eller forrige søndag om du vil.