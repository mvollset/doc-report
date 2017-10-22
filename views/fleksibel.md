---
layout: default
---
# Fleksibel viewet.

Fleksibel viewet er en pivot tabell visning av dataene. Det gir sluttbrukeren mulighet til å velge hvilke kolonner og rader det skal summeres på, og hvilke kolonne som skal summeres og hvordan data skal presenteres. Dette styres ved hjelp av opsjoner som settes i config fila.

Eksempel på config.js fil for en fleksibel rapport:
```js
report={
            pivotRows: ["Byrådsavdeling","Avdeling"],//Initielle rader i tabellen
            pivotCols: ["Prioritet"], //Initielle kolonner i tabellen 
            pivothiddenAttributes: ["Saksnummer", "Kort beskrivelse", "Produktgruppe", "Mottatt","_Status_sort","_Prioritet_sort"],//Attributter du ikke vil vise i pivot, men som er synlige i datagrunnlaget.
            pivotRenderer: "Tabell", //Visning ved oppstart
            pivotAggregator:"Antall",//Summerings funksjon ved oppstart
            zoomURL:'/report/fagsystem-zoom',//Zoom url kan utelates
            pivotincludeCount:true,//Skal det være en enkel teller som oppsummeringsfunksjon. Hvis datagrunnlaget er aggregert skal denne stå til false.
            hideUI:false //Viser kolonne og rad velger hvis den er satt til false.
    }
 module.exports=report;
```

## Aggregators

Hvis pivotincludeCount=true, så vil det bli lagt til en aggregator som teller, og en enkel summering av alle numeriske kolonner i datagrunnlaget. Ønsker du mer avanserte summeringsfunksjoner enn enkel summering, må dette legges inn pr rapport. Eksempler på avanserte aggregeringsfunksjoner er
 - avg 
 - sumOverSum
 - sumAsFractionOfTotal
 - countAsFractionOfRow
For å legge disse til må du legge til en aggregator property på report i config.js 

eks:
```js
aggregators:{
                  "Sum (Value)":function(){
                        return intSum(["value"]);
                  },
                   "Average":function(){
                        return avg(["value"]);
                  }
            } 
```


## Sortering

Hvis datagrunnlaget inneholder kolonner som starter med _ vil de ikke bli vist i datagrunnlaget, hvis det finnes en kolonne som heter _colname_sort og en som som heter _colname_ vil colname bli sortert i rekkefølgen til _colname_sort. Dette gjør det mulig å sortere remedy statuser etter statusverdi og ikke tekst.

