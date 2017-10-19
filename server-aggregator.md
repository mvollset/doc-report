# Server aggregering

Det er mulig å sette opp enkel aggregering av dataene. Dette gjøres i config.js fila til rapporten.
Eks:
```js
config={
    serverAggregator:{
        onchangeof:["Org"],
        includeTotals:false,
        "aggregators":[{"name":"tall","func":"sum","col":"tall"},{"name":"distribution","func":"distribution","col":"Cat 2"}]
    },
    aggregateInView:false,
    aggregateInline:true
}
module.exports=config
```

##serverAggregator
Dette er selve definisjonen av aggregatoren. onchangeof er et array med hvilke kolonner rapporten skal aggregeres på. Data må være ferdig sortert!!! 
 
 - _onchangeof_ - Array med kolonnenavn som det skal aggregater for
 - _includeTotals_ - true/false skal en regne ut slutt aggregat for hele datasettet.
 - _aggregators_ - hva og hvordan det skal aggregeres
 - _aggregateInView_ - skal datasettet aggregeres i view renderer eller må det aggregeres før. Hvis du bare skal vise en slutt sum, kan du spare en ekstra runde gjennom dataene ved å flytt aggregeringen til viewet.
 - _aggregateInline_ - true/false Skal aggregerings resultatet legges inn i selve datasettet, eller skal det bare sendes ved som _aggregate til viewet


## aggregators 
Disse bestemmer hvilken aggregerings funksjon som skal brukes, kolonne verdien skal hentes fra og hvilken kolonne resultatet skal til.
 - _name_ - Resultat kolonne/navn
 - _func_ - Aggregeringsfunksjon
 - _col_  - hvilken kolonne skal verdiene hentes fra

## Aggregeringsfunksjoner

### count
Teller antall rader.

### sum
Summerer verdien.

### average
Regner ut gjennomsnittet.

### max/min
Gir maximum eller minimums verdien.

### distribution
Gir antall av verdier innenfor en gruppe. Eks. Hvis du ønsker å telle antall saker pr prioritet på Gruppe Helpdesk.

##Retur datasett
Når du aggregerer vil det dukke opp et _aggregate objekt, det ser slik ut:
```js
[ { key: { value: 'AA', level: 1, col: 'dep' }, //Key angir hvilken kolonne som er endret, level hvilket nivå -1 er grand total, col hvilken kolonne som endrer seg.
    value: { Antall: 1 } }, //Value er en hash med en verdi pr aggregator
  { key: { value: 'AB', level: 1, col: 'dep' },
    value: { Antall: 1 } },
  { key: { value: 'A', level: 0, col: 'org' },
    value: { Antall: 2 } }
 .... ]
```













