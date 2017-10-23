---
layout: default
---
# Views
Et view er en presentasjon av data. Det bestemmer hvordan data skal vises til sluttbrukeren. Noen views forventer at datagrunnlaget er på en bestemt form.

## Datatables

Det er datatables.js som brukes for å implementere sortering i tabellene. Hvis du ønsker at en tabell skal være "sorterbar" må tabellen ha klassen sortable, da vil den automatisk bli gjort om til datatables tabell. 

>Det tar tid å laste en tabellen inn som en datatable, så prøv å unngå dette ved store datamengder.

## Kolonnevelger

Hvis en tabell er satt opp som en datatable, så er det mulig å implementere regler for hvilke kolonner som skal vises. Dette kan entes angis som en array med kolonne navn eller en funksjon. Dette angis som colvis i config.js fila til rapporten. Eks:
Array:
```js
colvis:{
    hide:["kol1","kol4"]
}
```

Funksjon, får kolonnenavnet som parameter, returner true for å skjule.
Eks: skjul kolonnen hvis den er angitt som parameter:
```js
colvis:{
    function(colname){
              var map={
                'Sakstype':'sakstype'
              };
              var mappedcol=map[colname];
              if(!mappedcol)
                return false;
              var k=$("#reportform input[name='" + mappedcol + "'],select[name='" + mappedcol + "']");
              if(!k.length)
                return false;
              var v=$(k[0]).val();
              if(v&&v!="--clear--"){
                return true;
              }
              return false;
            }
}
```

## Report - default viewet

Dette viewet vil liste skrive ut kolonnenavnene og så liste ut dataene rad for rad. Hvis det er definert noen renderer for en gitt kolonne i "configfile" vil denne renderen bli brukt.
```json
view:"report"
```

## Pivot

Dette viewet viser dataene i en pivot tabell, og en tabellutlisting av de samme dataene. 

```json
view:"fleksibel"
```

![Image of simplechart](/images/views/fleksibel.png)

## Enkel pivot øverst.
Forenklet pivot visning, viser oppsummering av data på en kolonne. Samme valg som Pivot

```json
view:"fleksibel-simple-ctop"
```

## Enkel pivot data øverst.
Forenklet pivot visning, viser oppsummering av data på en kolonne. Samme valg som Pivot
```json
view:"fleksibel-simple-ctop"
```

## Waterfall.
Waterfall chart som viser et balanseregnskap. Data må være på formen:
```js
[{
    d:"0-baseline",
    delta:123 //inngående balanse
    dim1:""
    },
    {
        d:"1-positive",
        delta:12 //Positive tall, - hvis negativt er positivt så putt det her
        dim1:"navn"
    },......
    {
        {
        d:"2-negative",
        delta:12 //Negative tall, - hvis negativt er positivt så putt det her
        dim1:"navn"
    },.......
]
```
![Image of simplechart](/images/views/waterfall.png)

## Simplechart
Viser en enkel graf med en datatabell under. Vil vise første tekstkolonne som x-akse og første numeriske kolonne som y verdi. Når du sorterer tabellen, vil grafen også bli sortert.
```json
view:"simple-ctop"
```
![Image of simplechart](/images/views/simple-ctop.png)

## Targets
Speedometer, gauge visning av mål. Data på formen:
```js
[
    {
        "name":"mål1",
        "actual":123 //Faktisk verdi
        "target":124 //Målet
    },
    ........
]
```
```json
view:"targets"
```
![Image of simplechart](/images/views/targets.png)

    


