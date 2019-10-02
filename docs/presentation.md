class: dark middle

# Databanken I
> Hoofdstuk 7 - Introductie **S**tructured **Q**uery **L**anguage

> Boek hoofstuk 7.3

---
### **S**tructured **Q**uery **L**anguage
# SQL

**Definitie**
- relationele gegevenstaal voor relationele database systemen 
- niet-procedurele taal
- industriestandaard

**Database managementsystemen**
- Oracle : PL/SQL 
- SQL Server : TRANSACT-SQL
- DB2 (IBM) 
- Informix
- Sybase–MySQL


---
### **S**tructured **Q**uery **L**anguage
# Overzicht
`SQL` bestaat uit **3** subtalen

**Data Definition Language (DDL)**
- Creatie van een database, en het definiëren van database objecten (tabellen, storedprocedures, views,...)
- `CREATE`, `ALTER`, `DROP`

**Data Manipulation Language (DML)**
- opvragen en manipuleren van de gegevens in een database
- `SELECT`, `INSERT`, `UPDATE`, `DELETE`

**Data Control Language (DCL)**
- gegevensbeveiliging en authorisatie
- `GRANT`, `REVOKE`, `DENY`

---
### **S**tructured **Q**uery **L**anguage
# Opmaak en commentaar
```sql
SELECT * FROM table  -- Selecteer alle kolommen van een tabel
```

```sql
SELECT * 
FROM TABLE 
    /*
        SELECTEER alle kolommen
        VAN tabel 
    */
```
> SQL is case insensitive (in de meeste gevallen)
> Enters en spaties tellen niet tenzij ze tussen single quotes **'**...**'** vallen.

1. **`--`** is een **inline-comment**, tekst op dezelfde lijn wordt in commentaar gezet.
2. **`/* */`**  is een **multi-line comment**, alle tekst ertussen wordt als commentaar gezien.

---
### **D**ata **M**anipulation **L**anguage 
# `SELECT`

```sql
SELECT [ALL | DISTINCT] {*|uitdrukking [,uitdrukking ...]}
FROM tabelnaam
[WHERE voorwaarde(n)] 
[ORDER BY {kolomnaam|volgnr}{ASC|DESC}[,...]
[LIMIT] [aantal]
```
- Bevragen/projecteren van data
    -  `SELECT`
- Filteren van data
    - `WHERE` 
- Sorteren van data 
    - `ORDER BY`
- Maximum aantal rijen
    - `LIMIT`
- Aggregaatsfuncties zoals `GROUP BY`, `AVERAGE`,`SUM`,...
    -  Zie hoofdstuk 8

---
### **D**ata **M**anipulation **L**anguage 
# `SELECT`
```sql
*SELECT [ALL | DISTINCT] {*|uitdrukking [,uitdrukking ...]}
*FROM tabelnaam 
[WHERE voorwaarde(n)] 
[ORDER BY {kolomnaam|volgnr}{ASC|DESC}[,...]
[LIMIT] [aantal]
```
- `SELECT`
    - ophalen van data uit 1 of meer kolommen
    - `DISTINCT` zorgt ervoor dat de getoonde rijen allen uniek zijn
        - OPM: `DISTINCT` is **performantie intensief**, het sorteert de volledige resultset, best te vermijden dus.
    - `*` selecteert alle kolommen
- `FROM`
    - geeft aan uit welke `tabel`/`view` of `subquery` de gegevens afkomstig zijn

> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_select.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_select1)

???
`SELECT *` heeft vaak nadelen voor de performantie.
---

### `SELECT`
# Voorbeeld
```sql
SELECT 
 `ProductID`
`,ProductName`
`,UnitPrice`
FROM Products
```
<img src="images\products-definition-diagram.jpg" alt="drawing" width="20%"/>
<img src="images\products-1.jpg" alt="drawing" width="51%"/>
> Enkel de gewenste **kolommen** worden weergegeven.
---

### `SELECT`
# Voorbeeld
```sql
SELECT `*` FROM Products
```
<img src="images\products-definition-diagram.jpg" alt="drawing" width="20%"/>
<img src="images\products-2.jpg" alt="drawing" width="100%"/>
> ***** is een alias voor alle kolommen.
---

### **D**ata **M**anipulation **L**anguage 
# `SELECT`
```sql
SELECT [ALL | DISTINCT] {*|uitdrukking [,uitdrukking ...]}
FROM tabelnaam 
*[WHERE voorwaarde(n)] 
[ORDER BY {kolomnaam|volgnr}{ASC|DESC}[,...]
[LIMIT] [aantal]
```
- `WHERE`
    - opgave van de voorwaarden waaraan de getoonde rijen moeten voldoen, ook wel de **filter** genoemd

> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_where.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_where1)

---

### `SELECT WHERE`
# Voorbeeld
```sql
SELECT 
 ProductID
,ProductName
,UnitPrice
FROM Products
`WHERE CategoryID = 1`
```
<img src="images\products-definition-diagram.jpg" alt="drawing" width="20%"/>
<img src="images\products-where.jpg" alt="drawing" width="35%"/>
> **Enkel** de producten van categorie 1 (beverages) worden weergegeven.

---

### `SELECT`
# `WHERE`
**Gebruik van literals **
- Numerische waarden: [...] `WHERE CategoryID = 1 `
- Alfanumerischewaarden: [...] `WHERE ProductName = ‘Chai’`
- Datums: [...] `WHERE OrderDate= '1996-07-04 00:00:00'`

**Vergelijkingsoperatoren** 
- =, >, >=, <, <=, <>
- Logische operatoren 
- Een interval van specifieke waarden 
- Een lijst van waarden 
- Onbekende waarden 
- Je kan haakjes gebruiken om de prioriteitsregels te doorbreken of het geheel leesbaarder te maken
- ...

---

### `SELECT WHERE`
# Voorbeeld
> Toon `productID`, `naam`, `aantal in stock` van de `producten` waarvan er **minder dan 5 in stock** zijn:


```sql
SELECT ProductID, ProductName, UnitsInStock 
FROM Products `WHERE unitsinstock < 5`
```

---

### `SELECT WHERE`
# Voorbeeld
> Toon `productID`, `naam`, `aantal in stock` van de `producten` waarvan de **naam begint met de letter 'A'**:


```sql
SELECT ProductID, ProductName, UnitsInStock 
FROM Products `WHERE ProductName >=  'A' and  ProductName < 'B'`
```

---

### `SELECT`
# `WHERE LIKE`
**Wildcards** 
- Zoeken naar patronen via `LIKE` | `NOT LIKE`
    - `%`  willekeurige tekenrij met 0 of meerdere tekens
        - `bl*` : **bl**, **bl**ack, **bl**ue, en **bl**ob
    - `_`  1 willekeurig teken
        - `h_t` : h**o**t, h**a**t, en h**i**t
    - `[]`  een teken tussen de haakjes
        - `h[oa]t` : h**o**t, h**a**t, maar niet hit
    - `^`   een teken dat niet in de haakjes zit
        - h[^oa]t  : h**i**t, maar niet hot of hat
    - `-`   een reeks tussen beide karakters of nummers
        -  `c[a-b]t` :  c**a**t en c**b**t, maar cct niet


> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_wildcards.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_wildcards1)

---

### `SELECT WHERE LIKE `
# Voorbeeld
> Toon `ProductID`, `naam`, `aantal in stock` van de `producten` waarvan de **naam begint met de letter 'A'**:

```sql
SELECT ProductID, ProductName, UnitsInStock 
FROM Products `WHERE ProductName LIKE 'A%'`
```

<hr/>
> Toon `ProductID`, `naam` van de `producten` waarbij de **tekenreeks 'anton' voorkomt in de naam**:


```sql
SELECT ProductID, ProductName 
FROM Products `WHERE ProductName LIKE '%anton%'`
```

---

### `SELECT WHERE`
#  LOGICAL OPERATOR
`OR` | `AND` | `NOT`
- Samenbundelen van filter criteria
- Net zoals in de wiskunde, kunnen er prioriteitsregeles toegevoegd worden via haakjes.


> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_and_or.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_where4)

---

### `SELECT WHERE` LOGICAL OPERATOR
# Voorbeeld
```sql
SELECT ProductID, ProductName, SupplierID, UnitPrice 
FROM Products 
WHERE `(`ProductName LIKE 'T%' OR ProductID = 46`)` 
      `AND` UnitPrice > 16.00
```

<hr/>

```sql
SELECT ProductID, ProductName, SupplierID, UnitPrice 
FROM Products 
WHERE    `(`ProductName LIKE 'T%'`)` 
          `OR` `(`ProductID = 46 AND  UnitPrice > 16.00`)`
```

---

### `SELECT WHERE`
# INTERVALS
`BETWEEN` | `NOT BETWEEN`
- Tussen 2 waarden
    - Gehele getallen
    - Decimale getallen
    - Data | Datums

> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_between.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_between1)

---

### `SELECT WHERE` INTERVALS
# Voorbeeld
> Selecteer de producten (naam en eenheidsprijs) waarvan de **eenheidsprijs tussen 10 en 15 euro (grenzen inbegrepen)**

```sql
SELECT ProductID, UnitPrice 
FROM Products 
WHERE UnitPrice `BETWEEN 10 AND 15`
```
---

### `SELECT WHERE`
# Range
- `IN` | `NOT IN`
    - Waarvan de kolom gelijk is aan een waarde uit een lijst van waarden.

> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_in.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_in1)

---

### `SELECT WHERE` Range
# Voorbeeld
> Geef productID, naam, supplierID van de producten die **geleverd worden door de leveranciers/suppliers met ID 1, 3 of 5**


```sql
SELECT ProductID, ProductName, SupplierID 
FROM Products 
WHERE SupplierID `IN (1,3,5)`
```
> Geef productID, naam, supplierID van de producten die **NIET** geleverd worden door de leveranciers/suppliers met ID 1, 3 of 5**


```sql
SELECT ProductID, ProductName, SupplierID 
FROM Products 
WHERE SupplierID `NOT IN (1,3,5)`
```
---

### `SELECT WHERE`
# `NULL`ables
- `IS NULL` | `IS NOT NULL`
    - `NULL` waarden komen voor wanneer er bij input in een bepaalde kolom **geen waarde** werd ingebracht en er geen standaardwaarde voor die kolom voorzien was. 
    - Een `NULL` waarde is **niet hezelfde als**
        - **0** (numerische waarden)
        - **blanco** (karakter waarden)
    - `NULL` velden worden onderling gelijk beschouwd (`DISTINCT`) 
    - Als in een rekenkundige uitdrukking een `NULL`-veld wordt is het resultaat ook `NULL`

> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_null_values.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_null1)

---

### `SELECT WHERE NULL`ables
# Voorbeeld
> Selecteer de leveranciers waarvan de regio **onbekend** is


```sql
SELECT Companyname, Region 
FROM Suppliers 
WHERE Region `IS NULL`
```
> Selecteer de leveranciers waarvan de regio **bekend** is

```sql
SELECT Companyname, Region 
FROM Suppliers 
WHERE Region `IS NOT NULL`
```
---

### `SELECT WHERE NULL`ables
# Caveats
Let op met `NULL`ables
> Geef alle leveranciers die niet in de regio 'LA' wonen.

```sql
SELECT companyname, region FROM suppliers 
WHERE region <> 'LA'
```

> Geef alle leveranciers die niet in de regio 'LA' wonen **of de regio niet geweten is.**

```sql
SELECT companyname, region FROM suppliers 
WHERE region <> 'LA' `OR region IS NULL`
```

> Let op, deze resultsets verschillen.

---
### The Billion Dollar Mistake
# `Null`ables

> "I call it my billion-dollar mistake. It was the invention of the null reference in 1965. At that time, I was designing the first comprehensive type system for references in an object oriented language (ALGOL W). My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years."

- Sir Charles Antony Richard Hoare

---

### **D**ata **M**anipulation **L**anguage 
# `SELECT`
```sql
SELECT [ALL | DISTINCT] {*|uitdrukking [,uitdrukking ...]}
FROM tabelnaam 
[WHERE voorwaarde(n)] 
*[ORDER BY {kolomnaam|volgnr}{ASC|DESC}[,...]
[LIMIT] [aantal]
```
- `ORDER BY`
    - Kan 1 of meerdere sorteervelden bevatten 
    -  Een sorteerveld kan gespecificeerd worden via de **kolomnaam**, of door het **volgnummer** op te geven dat overeenkomt met de volgorde van het gegeven achter de `SELECT` clausule (startend vanaf **1**)
    - Indien meerdere sorteervelden voorkomen, gebeurt het sorteren eerst op basis van het eerste veld, bij gelijkheid op basis van het tweede,... 

---
### **D**ata **M**anipulation **L**anguage 
# `SELECT`
```sql
SELECT [ALL | DISTINCT] {*|uitdrukking [,uitdrukking ...]}
FROM tabelnaam 
[WHERE voorwaarde(n)] 
*[ORDER BY {kolomnaam|volgnr}{ASC|DESC}[,...]
[LIMIT] [aantal]
```
- `ORDER BY`
    - Standaard gebeurt het sorteren in stijgende volgorde (volgens numerieke waarde, of volgens computercode bvb ASCII). Een dalende volgorde moet expliciet vermeld worden met `DESC`
    - Merk op dat het `ORDER BY` statement vaak het langste duurt.
        - Indien mogelijk is een `ORDER BY` clausule best te mijden wegens performantie.
    
> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_orderby.asp) | maak enkele [oefeningen](https://www.w3schools.com/sql/exercise.asp?filename=exercise_orderby1)
---

### `SELECT ORDER BY`
# Voorbeeld
> Toon een **alfabetische** lijst van de productnamen


```sql
SELECT ProductName 
FROM Products 
ORDER BY ProductName -- ASC is optioneel
```

> Ditmaal op basis van de **kolomnummer**.

```sql
SELECT ProductName 
FROM Products 
ORDER BY `1` -- ASC is optioneel
```

---

### `SELECT ORDER BY`
# Voorbeeld
> Toon productid,naam, categoryid en eenheidsprijs van de producten gesorteerd op categoryid. Indien binnen 1 categorie producten dezelfde prijs hebben, dan dient het product met de hoogste prijs bovenaan te staan.

```sql
SELECT 
 ProductID
,ProductName
,CategoryID
,UnitPrice 
FROM Products 
*ORDER BY 
* CategoryID
*,UnitPrice DESC
```
---

### **D**ata **M**anipulation **L**anguage 
# `SELECT`
```sql
SELECT [ALL | DISTINCT] {*|uitdrukking [,uitdrukking ...]}
FROM tabelnaam 
[WHERE voorwaarde(n)] 
[ORDER BY {kolomnaam|volgnr}{ASC|DESC}[,...]
*[LIMIT] [aantal]
```
- `LIMIT | TOP`
    - Bepaalt hoeveel rijen getoond moeten worden 
    - Best te gebruiken **in combinatie met `ORDER BY`**
        - Anders zou het kunnen dat je nooit dezelfde resultset krijgt.
    - Verschilt enorm per SQL Dialect
        - MySQL gebruikt `LIMIT` na de `ORDER BY` clausule
        - SQL Server gebruikt `TOP` direct na het keyword `SELECT`

> Bekijk de [documentatie](https://www.w3schools.com/sql/sql_top.asp)
---

### `SELECT LIMIT`
# Voorbeeld
> Selecteer de **eerste 5 producten** gesorteerd op ID

```sql
SELECT * FROM Products
ORDER BY ProductID
`LIMIT 5`
```

> Toon de productnamen van de **laatste 10 producten**,

```sql
SELECT ProductName 
FROM Products 
ORDER BY ProductID DESC
`LIMIT 10`
```
> De laatste 10 producten zijn diegene met de hoogste **ID**.
---
### `SELECT`
# Uitvoering vs Syntax
```sql
SELECT * FROM tabelnaam 
[WHERE voorwaarde(n)] 
[ORDER BY {kolomnaam|volgnr}{ASC|DESC}[,...]
```
- Jammer maar helaas, is de **volgorde van uitvoering** van het bovenstaande statement is **niet gelijk aan de syntax**.
- Het **einderesultaat** is de **cumul van meerdere tussenresultaten**.

### Volgorde van uitvoering:
1. Eerst wordt het `FROM` statement verwerkt.
2. Nadien gebeurt er een **filtering** of **selectie** door het `WHERE` statement
3. Vervolgens ondergaan de geselecteerde rijen een **projectie** door het `SELECT` statement
4. Tot slot worden de geprojecteerde rijen **gesorteerd** door het `ORDER BY` statement


---
### `SELECT`
# Oefeningen
1. Geef voornaam en familienaam van werknemers met code 54, die in een willekeurige afdeling werken met uitsluiting van afdeling D11. 
2. Geef nummer, naam en afdelingsnummer van alle werknemers met salaris tussen 15000 en 24000 en niveau tussen 17 en 20. 
3. Geef nummer, naam en opleidingsniveau van alle werknemers met niveau 16, 18 of 20. 
4. Geef nummer, naam van vrouwelijke werknemers waarvan familienaam start met een ‘S’ of ‘T’. 
5. Geef nummer, naam van alle werknemers met onbekende jobcode. 
6. Geef nummer, naam en afdelingsnummer van alle werknemers, waarvan de familienaam start met een P en die in een afdeling werken beginnend met D en als 3° karakter een 1 hebben.


---
### Alias
# TODO
- Alias
- Cast/Convert
- Berekeningen
- ...
