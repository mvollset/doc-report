---
layout: default
---
# Konfigurasjon av Stupid Reporting.

Konfigurasjon gjøres i, hovedsaklig, to filer. /config/config.js og i hvis den kjører under iisnode i web.config. Opsjonene i web.config er dokumentert her:
[IIS Node](https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config)

## Database forbindelse.
Det er to ting som må settes opp for å få koblet til en database.
config.db_module den skal være ora for oracle eller ms for ms-sql. Og så må du sette dbconfig, dette er "connection strengen" til databasen.

### MS SQL
```js
config.db_module="ms",
config.dbconfig={ user: "user",
    password: "password",
    database: "mydatabase",
    server: "dbserver1",
    connectionTimeout: 60000,
    requestTimeout: 120000,
    options: {
        instanceName: "data"
    }
}
```

### Oracle
```js
config.db_module="ora",
config.dbconfig={ user: "user",
    password: "password",
    database: "mydatabase",
    connectString:"server01/sidname"
}
```

## Flere databaseforbindelser
Det er også mulig å ha flere databaseforbindelser, de må settes opp slik:
```js
config.db_connections={
    "[connectionname]":{
        db_module:"ms",
        user: "user",
    password: "secret",
    database: "db",
    server: "db-server",
    connectionTimeout: 60000,
    requestTimeout: 120000
    },
    "json-test":{
        db_module:"web-json"
    }
}
```

I rapporter vil disse database forbindelsene refereres til slik:
```js
"connectionname":"json-test"
```



## CSV parametere
Dato biblioteket som brukes er moment, dokumentasjon på alle muligheter finnes her: [moment.js](http://momentjs.com/docs/#/displaying/format/)

```js
config.csvdelimiter = ";",
config.csvDateformat = "DD.MM.YYYY HH:mm",
```

## Andre parametere

```js

config.approot="/", //Rotkatalogen til applikasjonen, trenger ikke å være det samme som rota sett fra klientens side.....
config.reportdir = "reports/development", //Rotkatalogen til rapportene, alle under kataloger vil bli skannet for rapporter.
config.reportdir = "reports/reportdefinition",
config.customviewdir = "reports/views",
//Filer i denne mappa vil bli kopiert til custom-views mappa. Viewene vil være tilgjengelige som custom-views/filnavn
config.customjsdir = "reports/javascript",
//Filer i denne mappa vil bli kopiert til public/javascripts/custom-scripts. Disse er da tilgjengelige i views som approot/javascscripts/custom-scripts/filnavn
config.homepageview="reports-liste",//Hvis du ønsker et eget view på førstesiden
//Antall kolonner i førsteside viewet.
config.reportlistcols=4,
//Det som ligger i reportconfig blir sendt videre ut til klienten.
config.reportconfig = {
    jdateFormat: "dd.mm.yy", 
    mdateFormat: "DD.MM.YYYY", 
    mdateFormatLong: "DD.MM.YYYY HH.mm",
    approot:"/"
}
```

## Windows Authentication med IIS node.
Hvis du ønsker å skru på dette må du skru på windows authentication på IIS. I web.config i SR mappa må du angi promoteServerVars dette skal være minimum AUTH_USER. F.eks.
```xml
<iisnode promoteServerVars="AUTH_USER,AUTH_TYPE"/>
```

Dette vil fortelle IIS at den skal sende autentisert brukernavn til SR. I config.js må du sette følgende verdier:
```js
config.iisnodeauth = true,
config.dummyauth = false,
config.stripDomain=[true/false]
```
Du kan nå styre tilgangen til rapport applikasjonen med windows grupper som du ville i en standard web applikasjon. 

## Sessionstore
Det er anbefalt å sette opp en session store. Denne lagrer sesjonsinformasjon om påloggede brukere. Det følger med en for oracle, og en for SQL server. Det er mulig å laste ned for de fleste tenkelige og utenkelige datakilder. Dette settes i config.session_config og config.sessionstoreConfig.
Eksempel session config
```js
config.session_config = {
    secret: "shhh secret",
    resave: false,
    saveUninitialized: true,
    cookie: {
        secure: false
    },
    name: "report-engine",
    store:"connect-mssql" //Denne vil bruke ms-sql som session store. Vil du bruke oracle skriver du connect-ora. Hvis du ikke angir noe vil det brukes en in memory store, som ikke fungerer godt i produksjonsmiljøer.
}
```

### Eksempel på oracle session oppsett
Session store config skal ha samme verdier som en database connection.

```js
config.session_config = {
    secret: "shhh secret",
    resave: false,
    saveUninitialized: true,
    cookie: {
        secure: false
    },
    name: "report-engine",
    store:"connect-ora" //Denne vil bruke ms-sql som session store. Vil du bruke oracle skriver du connect-ora. Hvis du ikke angir noe vil det brukes en in memory store, som ikke fungerer godt i produksjonsmiljøer.
},
config.dbconfig={ user: "user",
    password: "password",
    database: "mydatabase",
    connectString:"server01/sidname"
}
```
## Accessprovider
En access provider gir mer finkornet tilgangsstyring enn kun windows autentisering. Det er foreløpig laget to:
 - remedyprovider for ms-sql 
 - remedyprovider for oracle.
Begge disse bruker db konfigurasjonen fra rapportmotoren, så det eneste du trenger å angi er 

```js
config.accessprovider="remedyoraprovider" // eller remedymsprovider
```

Disse forventer at det finnes to views i databasen. Et for å hente ut brukerinformasjon og et for å hente tilgangene til rapportene.

### rpe_userview
Dette skal inneholde 
- userid
- name
- email
- groups Som angitt i user_x formen, kan selvsagt hente fra CTM people også.

### report_access
Dette skal ha kolonnene:
 - report Navnet på rapporten
 - principal -Gruppe eller brukerid som har tilgang til rapporten

Hvordan dette er laget i remedy er opp til hver enkelt, dette er for å gjøre det mulig å lage rapporter for evt. kunder og tredjeparter m.m.



