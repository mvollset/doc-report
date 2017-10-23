---
layout: default
---
# Reportconfig

En av de viktigste funksjonene i rapportmotoren er muligheten til å definere egen funksjonalitet i reportconfig. Denne blir lest inn sammen med rapporten og så vil funksjoner og innstillinger være tilgjengelig
både i renderere på serversiden og i javascript på klient siden. 

Noe av de tingene som kan legges inn er view spesifikt, altså avhengig av hvilket view du velger så må/kan du legge inn ting i report config.

### onload
Hvis du lager en funksjon som heter onload vil denne bli kjørt når rapporten er ferdig å laste. 
```js
config = {
   onload:function(){
       alert('onload');
   }

}
module.exports = config;
```

### hcConfig

Hvis du har en highcharts graf på siden kan du angi ekstra opsjoner til grafen i hcConfig objektet i chartConfig
```
 chartConfig: {
        nameCols: ['kroner', 'timer'],
        categoryCol: 'fulltnavn',
        categoryIsMonth: false,
        title: 'Loggede timer/kroner pr konsulent denne måned.',
        stacking: false,
        accumulated: false,
        series: {
            "timer": {
                yAxis: 1
            }
        },
        hcConfig: {
            yAxis: [{ // Primary yAxis
                labels: {
                    format: '{value} kr'
                },
                title: {
                    text: 'Verdi'
                }
            }, { // Secondary yAxis
                title: {
                    text: 'Antall timer'
                },
                labels: {
                    format: '{value} timer'
                },
                opposite: true
            }],
            tooltip: {
                shared: true
            }
        }
    }
```

### Datatable config

Her kan du sette ekstra opsjoner for datatable.
```

    datatableConfig: {
        searching: false,
        order: [],
        columns: [{
            "data": "fulltnavn",
            "title": "Konsulent"
        }, {
            "data": "kroner",
            "title": "Kroner",
        }, {
            "data": "timer",
            "title": "Timer"
        }]
    }
    
```

### colvis

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