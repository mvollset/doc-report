---
layout: default
---
# Getting stupid

Hvordan komme i gang med Simple Report Engine.

## Sette opp et utviklingsmiljø
>Det er lurt å ha en database med noe data i som en du kan utvikle rapportene dine mot. For å komme igang er det enkleste å bruke SQL server.

 1. Installer node.js velg LTS relasen
 2. Installer [github for windows](https://desktop.github.com/)
 3. Installer grunt client, kjør "npm install -g grunt-cli" fra kommandolinja.
 4. Bruk en god teksteditor, du bør kunne se forskjell på tab og space, og håndtere UTF-8 på en god måte. (Notepad duger ikke!!).
 5. 

### Lag din første rapport.
Start med å sette opp forbindelsen til databasen i settings.json fila. Du kan kopiere settings-sample-ms.json til settings.json så er det meste på plass. Skal du koble opp mot mssql, da må du sette  db_module="ms", sett report dir til den mappen du vil ha rapportene dine i. f.eks utvikling. Og videre det som trengs for å koble til.
```json
    {
    "db_module": "ms",//Lovlige verdier er ora for Oracle, pg for postgresql ms for sql server
    "locales": [{
        "English": "en"
    }, {
        "Norsk": "no"
    }],
    //Parametere for csv export
    "csvdelimiter": ";",
    "csvDateformat":"DD.MM.YYYY HH:mm",
    "morganformat": "combined",
    //Hvilken mappe skal rapportdefinisjonene leses fra. Alle undermapper leses også.
    "reportdir":"reports/utvikling",
    "dbconfig": {
        "user": "db-bruker",
        "password": "passord",
        "database": "test",
        "server": "dbserver",
        "connectionTimeout": 15000,
        "requestTimeout":60000,
        "options": {
            "instanceName": "inst"
        }

    }.........
```
### rapport.json
<blockquote class="tip">Legg alle filene til en rapport i en egen mappe.</blockquote>


Opprett en fil i reports/utvikling katalogen som heter rapport.json som inneholder dette:
```json
   {
    "name":"rapport", 
    "title":"Min første rapport", 
    "sqlfile":"rapport.sql",
    "show":true
 }
```
Og opprett en fil som heter rapport.sql som inneholder:

    select CategoryID,Name,Level,Enabled,ParentType,Index from Category

Start Rapport motoren, ved å skrive grunt når du står i rot mappa til applikasjonen. Gå til 
http://localhost:3000 , 
![Hjemmesiden](https://drive.google.com/file/d/0B8FTEkm8JzzSeFFyWkQ2UTRtMFk/view?usp=sharing)
førstesiden vil da vise en link til rapporten din. Klikk på linken å rapporten vil kjøre. 
![Første rapport](https://drive.google.com/file/d/0B8FTEkm8JzzSQVJmdG5RMWI4SEE/view?usp=sharing)
>En rapport som har nok parametere til å kjøre vil kjøre automatisk når du åpner den. 
Hvis vi ønsker å pynte litt på lista så kan vi f.eks. sortere etter nivå, og ParentType,

```sql
select CategoryID,Name,Level,Enabled,ParentType,Index from Category 
ORDER BY Level,ParentType
```

Vi ønsker kun å vise de kategoriene som gjelder for "Incident", dette kan vi gjøre på to måter, enten ved å legge til et parameter, eller hardkode det inn i SQL spørringen. Vi kan gjøre det ved å legge opp et parameter. Lukk opp rapport.json og legg til parameter definisjonen:

```json
  {
    "name": "rapport",
    "title": "Min første rapport",
    "sqlfile": "rapport.sql",
    "sqlfile": "rapport2.sql"
    "show": true,
    "parameters": [{
        "name": "parenttype", //Dette er navnet som dukker opp i url'en og sql spørringen
        "prompt": "Gjelder for", //Ledeteksten 
        "required": true,
        "type": "string", //Type
        "operator": "=",
        "value": "SupportPoint.Incident",
        "fixed": true
    }]
}
```

Så endrer vi spørringen til å bruke parameteret:

```sql
    select CategoryID,Name,Level,Enabled,ParentType,Index from Category
    WHERE ParentType=@parenttype 
    ORDER BY Level,ParentType
```

>Parametere som ikke begynner med _ vil bli overført til spørringen med navnet som er definert i name!

Hvis vi vil at brukeren skal kunne velge parenttype så må vi legge opp opsjoner på parameteret og si at det ikke er fixed.

```json
     {
    "name": "rapport3",
    "title": "rapport v3",
    "sqlfile": "rapport.sql",
    "show": true,
    "parameters": [{
        "name": "parenttype",
        "prompt": "Gjelder for",
        "required": true,
        "type": "string",
        "operator": "=",
        "options":[
        {"value":"SupportPoint.Incident","markup":"Incident"},
        {"value":"SupportPoint.Change","markup":"Change"},
        {"value":"SupportPoint.Asset","markup":"Asset"}
        ],
        "fixed": false
    }]
}
}
```

>Options objekter har både en markup og en value property, markup er det som vises, og value er verdien.

>Alle filer skal lagres som UTF-8!!!

### Layouts
SRE bruker jade som layout engine. Det anbefales å lese litt om [Jade](http://jade-lang.com) før en begynner å endre på viewene. De er lagt opp som følger:
#### layout.jade - Dette er grunn malen
Inneholder følgende seksjoner:

 block additionalhtmlhead - kan brukes hvis en ønsker å legge inn ekstra html i html head
 block content - Her kommer innholdet.
 block footerscripts - overstyr for å legge inn javascript spesifikt for dette viewet
#### custom-view/layout-user.jade
 Denne er lagt inn slik at vi har et sted vi kan endre grunn layouten på rapport sidene, den er arver layout
 
#### report-template.jade
Dette er default rapport malen. Den arver layout-user, og deler content blokken opp i følgende blocker:
 - reportheader
 - parameter Her skal parameterene inn
 - dataheader - antall rader funnet f.eks
 - data Her kommer selve dataene
 - datafooter her ligger link verktøyet
 - footer, denne er overstyrt og tom.
    .

### Treesample 
Denne rapporten henter alle incidents med en gitt kategorisering fra Remedy ITSM.

_Spørring_
```sql
select Incident_Number,Organization [Org],Department [Dep],[Site],Categorization_Tier_1 [Cat 1],
Categorization_TIer_2 [Cat 2],Categorization_Tier_3 [Cat 3] FROM HPD_HELP_DESK
WHERE 1=1
```
treesample.json fil:

```json
{
    "name":"treesample", //navn på rapport, url til rapporten blir da http://server/report/treesample
    "title":"Tree picker sample report", //Tittel på rapporten
    "sqlfile":"treesample.sql", //navn på sql fil 
    "show":true, //Skal vises på forsiden
    "view":"report", //Bruk view som er definert i report.jade
    "parameters": [{
    "name": "cat1",
    "prompt": "Categorisation Tier 1",
    "required": true,
    "type": "select",   //Standard dropdown liste
    "operator": "=",
    "sql":" AND [Categorization_Tier_1]  = @cat1",
    "paramsql":"select Distinct Categorization_Tier_1 [value],Categorization_Tier_1[markup] from Cfg_Service_Catalog_Lookup WHERE Help_Desk_Selection=0 AND Status_Cat=1" //Sql for å hente opsjonene. Dette skjer før første siden vises.
    },{
    "name": "cat2",
    "prompt": "Categorisation Tier 2",
    "depends":["cat1"], //Denne dropdown lista får ikke verdier før cat1 er fylt inn
    "required": false,
    "type": "select",   
    "operator": "=",
    "sql":" AND [Categorization_Tier_2]  = @cat2",
    "paramsql":"select Distinct Categorization_Tier_2 [value],Categorization_Tier_2[markup] from CFG_Service_Catalog_Lookup WHERE Help_Desk_Selection=0 AND Status_Cat=1 and Categorization_Tier_1=@cat1"//Legg merke til at sql'en må bruke parameter verdien fra det parameteret den er avhengig av.
    },{
    "name": "cat3",
    "prompt": "Categorisation Tier 3",
    "depends":["cat1","cat2"],
    "required": false,
    "type": "select",   
    "operator": "=",
    "sql":" AND [Categorization_Tier_3]  = @cat3",
    "paramsql":"select Distinct Categorization_Tier_3 [value],Categorization_Tier_3[markup] from CFG_Service_Catalog_Lookup WHERE Help_Desk_Selection=0 AND Status_Cat=1 and Categorization_Tier_1=@cat1 and Categorization_Tier_2=@cat2"
    }
   ]
 }
       

```
