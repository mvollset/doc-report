---
layout: posts
---
# Parametere


## name 
Navn på parameteret, hvis navnet starter med _ vil det ikke bli overført til sql spørringen, hvis ikke vil det være tilgjengelig i spørringen som @name på mssql og :name på oracle.

## prompt
Spørretekst ut til brukeren.

## required [true/false]
Om parameteret må være fylt ut for at rapporten skal kjøre eller ikke.

## type [select,mulitselect,autocomplete,date,datetime] 
Type parameter.

## operator [=,in list,between,like....]
Denne brukes til litt forskjellig, men hvis operator er between vil det overføres to parametere til sql spørringen name_to og name_from.

## fixed [true/false]
Kan brukeren endre parameteret eller ikke.

## value
Initiell verdi på parameteret, må være satt hvis fixed er true.

## sql
For paramtere som ikke er påkrevd, er dette sql'en som blir lagt til rapportens sql når parameteret er satt.

## options
Array med opsjoner for select/multiselect parametere.
```json
options:[{markup:"Brille",value:"brille"},
    ....]
```

## paramsql
Sql spørring for å hente opsjoner fra databasen, i stedet for å angi dem i options paramteret. Spørringen må returnere en markup og en value kolonne.

## depends
Array med navn på parametere dette parameteret er avhengig av. Det er da mulig å referere til evt. parametere i paramsql:
```json
depends:["param1"],
paramsql:"SELECT markup, value FROM dual where value1=:param1";
```
## attributes
Object med attributter som blir overført til html elementet, eks placeholder url for autocomplete og validering.

## validationMessage
Tekst som skal vises når parameteret ikke er gyldig. For paremetere med operator between er dette et object med verdier *name*_from og *name*_to
```json
 "name": "reported",
            "operator": "between",
            "validationMessage": {
                "reported_from": "Dato fra må fylles i (dd.mm.yyyy)",
                "reported_to": "Dato til må fylles i (dd.mm.yyyy)"
            }
```
