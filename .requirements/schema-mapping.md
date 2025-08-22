# LondonTaxi Schema Mapping & Development Guide 📋

## Tabellenübersicht

### Faktentabelle
- **Trip** - Zentrale Faktentabelle mit allen Taxi-Fahrten
  - Primäre Umsatzspalten: `FareAmount`, `TipAmount`, `TotalAmount`
  - Zahlungsart: `PaymentType` (String-Werte)
  - Standort-Keys: `PickupGeographyID`, `DropoffGeographyID`
  - Zeit-Keys: `DateID`, `PickupTimeID`, `DropoffTimeID`
  - Fahrzeug-Keys: `MedallionID`, `HackneyLicenseID`

### Dimensionstabellen

#### Date (Zeit-Dimension)
- **DateID** (Key) → Beziehung zu Trip[DateID]
- Verfügbare Zeit-Attribute: Year, Month, Quarter, DayName, etc.
- Ersetzt das generische Calendar-Template

#### Geography (Standort-Dimension)  
- **GeographyID** (Key) → Beziehung zu Trip[PickupGeographyID/DropoffGeographyID]
- Verfügbare Standort-Attribute: City, County, State, Country, ZipCode
- **Hinweis**: Kein Borough-Feld verfügbar, verwende County als Alternative

#### Time (Tageszeit-Dimension)
- **TimeID** (Key) → Beziehung zu Trip[PickupTimeID/DropoffTimeID]  
- Verfügbare Zeit-Attribute: HourNumber, DayTimeBucket, HourlyBucket

#### Medallion & HackneyLicense (Fahrzeug-Dimensionen)
- **MedallionID** (Key) → Beziehung zu Trip[MedallionID]
- **HackneyLicenseID** (Key) → Beziehung zu Trip[HackneyLicenseID]
- Fahrzeug-Codes und Lizenzdaten

#### Weather (Wetter-Dimension, optional)
- **DateID + GeographyID** → Zusätzliche Analyse-Dimension
- Wetterdaten: PrecipitationInches, AvgTemperatureFahrenheit

## Business Requirements Mapping

### TAXI-001: Total Revenue
```dax
[Total Revenue] = SUM(Trip[FareAmount]) + SUM(Trip[TipAmount])
```

### TAXI-002: Revenue Growth  
```dax
[Revenue YoY Growth %] = 
DIVIDE(
    [Total Revenue] - CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Date[Date])),
    CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Date[Date]))
)
```

### TAXI-003: Location Ranking
```dax
[Location Revenue Rank] = 
RANKX(
    ALL(Geography[City]), 
    [Total Revenue], 
    , 
    DESC
)
```

## Visuelle Anforderungen Anpassungen

### VISUAL-003 & VISUAL-005: Borough → County
- **Original**: "borough slicer" 
- **Tatsächlich**: Geography[County] oder Geography[City] verwenden
- **Drill-through**: City → County → State → Country

### VISUAL-004: Payment Types
- **Datenquelle**: Trip[PaymentType] (String-Spalte)
- **Keine separate Dimension**: Direkt aus Trip-Tabelle

## Star-Schema Design

```
          Date                    Geography
         ┌─────┐                  ┌─────────┐
         │DateID│◄────────────────┤GeographyID│
         └─────┘                  └─────────┘
             ▲                         ▲
             │                         │
             │                         │
         ┌───▼───┐                     │
         │  Trip │─────────────────────┘
         └───┬───┘                     
             │                         
             │                         
         ┌───▼───┐                 ┌──────────┐
         │ Time  │                 │Medallion │
         └───────┘                 └──────────┘
```

## Parameter-Definition

```tmdl
expression Parameter_Server = "if7l2k6jajhexaligopzadcdde-jqdtzkxghetune2a4f7mxsbodq.datawarehouse.fabric.microsoft.com" meta [IsParameterQuery=true, Type="Text", IsParameterQueryRequired=true]

expression Parameter_Database = "LondonTaxi" meta [IsParameterQuery=true, Type="Text", IsParameterQueryRequired=true]
```

## TMDL Naming Conventions

### Tabellennamen (lowercase)
- trip (Faktentabelle)
- date (Datums-Dimension)  
- geography (Standort-Dimension)
- time (Zeit-Dimension)
- medallion (Fahrzeug-Dimension)
- hackneylicense (Lizenz-Dimension)

### Measure-Namen
- [total revenue] - Basis-Umsatz
- [total revenue (ly)] - Vorjahres-Umsatz
- [revenue yoy growth %] - Jahreswachstum
- [revenue mom growth %] - Monatswachstum
- [location revenue rank] - Standort-Ranking
- [trip count] - Anzahl Fahrten

## Entwicklungsreihenfolge

1. ✅ **Schema validiert** - AzureSQLSchema.csv aktualisiert
2. ✅ **Requirements korrigiert** - Mapping erstellt
3. 🔄 **PBIP-Struktur erstellen** - TaxiRevenue.SemanticModel
4. 🔄 **Tabellen implementieren** - TMDL-Dateien
5. 🔄 **Beziehungen definieren** - relationships.tmdl
6. 🔄 **Measures entwickeln** - DAX-Formeln
7. 🔄 **Report erstellen** - Power BI Visuals
8. 🔄 **Validierung** - BPA ausführen
