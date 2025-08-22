# TaxiRevenue - Entwicklungsspezifikation 📊

## Projektübersicht
**Semantic Model**: TaxiRevenue  
**Datenquelle**: Microsoft Fabric Data Warehouse - LondonTaxi  
**Zielsetzung**: Taxi-Umsatzanalyse mit interaktiven Power BI Reports

## Datenmodell-Architektur

### Star-Schema Design
```
Date ────┐
         ├──► Trip ◄──── Geography
Time ────┘     │
               ├──── Medallion  
               └──── HackneyLicense
```

### Haupttabellen

#### 🎯 **trip** (Faktentabelle)
**Zweck**: Zentrale Faktentabelle mit Taxi-Fahrten und Umsatzdaten
**Wichtige Spalten**:
- `FareAmount` (decimal) - Grundfahrpreis
- `TipAmount` (decimal) - Trinkgeld
- `TotalAmount` (decimal) - Gesamtbetrag
- `PaymentType` (varchar) - Zahlungsart (Cash, Credit Card, etc.)
- `DateID` (int) → Beziehung zu date[DateID]
- `PickupGeographyID` (int) → Beziehung zu geography[GeographyID]
- `DropoffGeographyID` (int) → Beziehung zu geography[GeographyID]

#### 📅 **date** (Dimension)
**Zweck**: Datums- und Zeit-Dimension für Trendanalysen
**Beziehung**: date[DateID] ← trip[DateID] (1:n)
**Wichtige Spalten**: Year, Month, Quarter, DayName, MonthName

#### 🌍 **geography** (Dimension)  
**Zweck**: Standort-Dimension für geografische Analysen
**Beziehung**: geography[GeographyID] ← trip[PickupGeographyID] (1:n)
**Wichtige Spalten**: City, County, State, Country

#### ⏰ **time** (Dimension)
**Zweck**: Tageszeit-Dimension für zeitbasierte Muster
**Beziehung**: time[TimeID] ← trip[PickupTimeID] (1:n)  
**Wichtige Spalten**: HourNumber, DayTimeBucket, HourlyBucket

## Business Measures

### Core Revenue Measures
```dax
/// Gesamtumsatz aus Fahrtpreis und Trinkgeld
[total revenue] = SUM(trip[FareAmount]) + SUM(trip[TipAmount])

/// Anzahl der Fahrten
[trip count] = COUNTROWS(trip)

/// Durchschnittlicher Umsatz pro Fahrt  
[average revenue per trip] = DIVIDE([total revenue], [trip count])
```

### Time Intelligence Measures
```dax
/// Umsatz des Vorjahres
[total revenue (ly)] = CALCULATE([total revenue], SAMEPERIODLASTYEAR(date[Date]))

/// Jahr-zu-Jahr Wachstum in Prozent
[revenue yoy growth %] = 
DIVIDE(
    [total revenue] - [total revenue (ly)],
    [total revenue (ly)]
) * 100

/// Umsatz des Vormonats
[total revenue (pm)] = CALCULATE([total revenue], PREVIOUSMONTH(date[Date]))

/// Monat-zu-Monat Wachstum in Prozent
[revenue mom growth %] = 
DIVIDE(
    [total revenue] - [total revenue (pm)],
    [total revenue (pm)]
) * 100
```

### Ranking Measures
```dax
/// Standort-Ranking nach Umsatz
[location revenue rank] = 
RANKX(
    ALL(geography[City]), 
    [total revenue], 
    , 
    DESC
)
```

## Power BI Report Design

### Dashboard Layout (F-Pattern)
```
┌─────────────────────────────────────────────────┐
│  🎯 KPI Cards Row                               │
│  [Total Revenue] [YoY Growth] [MoM Growth]      │
├─────────────────────────────────────────────────┤
│  📈 Revenue Trend Chart    │ 📍 Top Locations   │
│  (Line Chart)              │ (Horizontal Bar)    │
├─────────────────┬───────────────────────────────┤  
│ 🍩 Payment Mix  │ 🎛️ Interactive Filters        │
│ (Donut Chart)   │ • Date Slicer                │
│                 │ • City/County Slicer          │
│                 │ • Payment Type Filter         │
└─────────────────┴───────────────────────────────┘
```

### Visual Spezifikationen

#### VISUAL-001: Executive KPI Cards
- **Total Revenue**: Conditional formatting (green/red based on growth)
- **YoY Growth %**: Sparkline with trend indicator  
- **MoM Growth %**: Comparison with target line

#### VISUAL-002: Revenue Trend Line Chart
- **X-Axis**: date[Date] mit Drill-down (Year → Month → Day)
- **Y-Axis**: [total revenue]
- **Series**: Current Year vs Previous Year
- **Features**: Reference lines, zoom slider

#### VISUAL-003: Top Locations Horizontal Bar Chart
- **Axis**: geography[City] (Top 10)
- **Values**: [total revenue]
- **Conditional Formatting**: Color scale based on revenue
- **Drill-through**: City → County → State

#### VISUAL-004: Payment Method Donut Chart  
- **Legend**: trip[PaymentType]
- **Values**: [total revenue]
- **Labels**: Percentage and absolute values
- **Drill-through**: Detailed payment analysis

#### VISUAL-005: Interactive Controls
- **Date Slicer**: Between-type mit date[Date]
- **Geography Slicer**: Multi-select mit geography[City/County]
- **Payment Filter**: Dropdown mit trip[PaymentType]

## Technische Implementation

### Parameter Configuration
```tmdl
expression Parameter_Server = "if7l2k6jajhexaligopzadcdde-jqdtzkxghetune2a4f7mxsbodq.datawarehouse.fabric.microsoft.com" meta [IsParameterQuery=true, Type="Text", IsParameterQueryRequired=true]

expression Parameter_Database = "LondonTaxi" meta [IsParameterQuery=true, Type="Text", IsParameterQueryRequired=true]
```

### Power Query Patterns
```powerquery
let
    Source = Sql.Database(#"Parameter_Server", #"Parameter_Database", [CreateNavigationProperties=false]),
    TripTable = Source{[Schema="dbo",Item="Trip"]}[Data],
    SelectColumns = Table.SelectColumns(TripTable, {
        "DateID", "FareAmount", "TipAmount", "TotalAmount", 
        "PaymentType", "PickupGeographyID", "DropoffGeographyID"
    })
in
    SelectColumns
```

### Relationship Definitions
```tmdl
relationship 'trip-date' = trip[DateID] -> date[DateID]
relationship 'trip-pickup-geography' = trip[PickupGeographyID] -> geography[GeographyID]  
relationship 'trip-dropoff-geography' = trip[DropoffGeographyID] -> geography[GeographyID]
relationship 'trip-medallion' = trip[MedallionID] -> medallion[MedallionID]
```

## Validierung & Qualitätssicherung

### Pre-Development Checklist
- ✅ Schema validiert (AzureSQLSchema.csv)
- ✅ Requirements korrigiert und gemappt
- ✅ PBIP-Struktur definiert
- ✅ Calendar-Template verfügbar

### Post-Development Validation
- [ ] BPA-Analyse ausführen (`.bpa/bpa.ps1`)
- [ ] Alle Measures getestet
- [ ] Parameter-Funktionalität validiert
- [ ] Cross-filtering getestet
- [ ] Performance optimiert

### Success Criteria
1. **Functional**: Alle TAXI-001 bis TAXI-003 Requirements erfüllt
2. **Visual**: Alle VISUAL-001 bis VISUAL-005 Requirements implementiert  
3. **Technical**: PBIP best practices befolgt
4. **Performance**: Schnelle Rendering-Zeiten
5. **Usability**: Intuitive Navigation und Interaktion

## Nächste Schritte
1. 🔄 PBIP-Projekt-Struktur erstellen
2. 🔄 TMDL-Tabellen implementieren  
3. 🔄 DAX-Measures entwickeln
4. 🔄 Power BI Report gestalten
5. 🔄 Testing und Optimierung
