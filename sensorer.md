# BMW E36 M50 Turbo – Sensorplan

Den här filen beskriver planerad sensorinstrumentering för BMW E36 med turbo-M50 och MaxxECU Track Gen 2.

Målet är inte bara att skydda motorn med fasta gränsvärden, utan att kunna följa hur motorn reagerar när cylinderlast och varvtal successivt höjs. Fokus ligger därför på trenddata: bränsletrycksdifferens, vevhustryck, kylsystemtryck, EMAP/MAP, knock, EGT och oljetryck.

---

## Sensoröversikt

| Mätning | Föreslagen sensor | Mätområde / typ | Placering |
|---|---|---:|---|
| Knock | Befintliga BMW-knacksensorer | 2 st | Originalplacering på block |
| Oljetryck | Honeywell MIP ~10 bar eller Bosch PSS-10 | 0.5–4.5 V | Oljegalleri / filterhus |
| Oljetemperatur | Bosch NTC M12 | NTC | Oljegalleri / filterhus / sump |
| Bränsletryck | Bosch PSS-10 eller motsvarande E85-kompatibel 10 bar | 0.5–4.5 V | Fuel rail |
| Bränsletemperatur | Bosch NTC M12 | NTC | Fuel rail eller retur direkt efter rail/regulator |
| Vevhustryck | Bosch 0 261 230 216 | 0.1–1.15 bar absolute | Separat tryckport i ventilkåpan |
| Kylsystemtryck | Honeywell MIP 4 bar absolute | 0.5–4.5 V | Kylsystem, helst motor-/toppsida |
| EMAP | Honeywell MIP 8 bar absolute | 0.5–4.5 V | Före turbo, fjärrmonterad |
| EGT | K-type thermocouple | Pre-turbine | Samlad EGT före turbo |
| Bränslepumpsspänning | Ingen separat sensor | Spänningsdelare till AIN | Mät pumpens +12 V vid pumpen |

---

# 1. Knock

Befintliga dubbla knacksensorer kopplas in till MaxxECU.

Prioritet:
- konfigurera båda sensorerna
- logga normal mekanisk ljudnivå över RPM
- bygga knock-threshold från den egna motorn
- använda knock som både logg- och skyddsparameter

Knock bör vara inkopplat innan laddtryck och cylinderlast börjar höjas på allvar.

---

# 2. Oljetryck

## Föreslagen sensor

- Honeywell MIP omkring 10 bar
- alternativt Bosch PSS-10

Krav:
- 5 V matning
- 0.5–4.5 V signal
- automotive/industrial robusthet
- kompatibel med motorolja

Oljetrycket bör användas med RPM-beroende minimumgräns snarare än ett enda fast cut-värde.

De faktiska gränserna bestäms först efter att varm normalmotor har loggats.

---

# 3. Oljetemperatur

## Föreslagen sensor

Bosch NTC M12.

MaxxECU Track Gen 2 har intern pull-up på lämpliga analoga ingångar, så en extern motståndslösning behövs normalt inte.

Oljetemperatur används tillsammans med oljetryck eftersom ett tryckvärde utan temperaturkontext är betydligt mindre användbart.

---

# 4. Bränsletryck

## Föreslagen sensor

Bosch PSS-10 eller motsvarande E85-kompatibel tryckgivare omkring 10 bar.

Det viktiga värdet är inte bara rail pressure utan injektorernas differenstryck.

Om bränsletrycksgivaren är absolut:

```text
Fuel Delta P = Fuel Pressure Absolute - MAP Absolute
```

Om givaren är gauge:

```text
Fuel Delta P = Fuel Pressure Gauge + BARO - MAP Absolute
```

För nuvarande regulator är målet ungefär 3.3 bar differenstryck.

Fuel Delta P bör loggas mot:
- RPM
- MAP
- injector duty
- fuel temperature
- pump voltage

---

# 5. Bränsletemperatur

## Föreslagen sensor

Bosch NTC M12.

Placering:
- helst i fuel rail
- alternativt i returen direkt efter rail/regulator

Undvik att mäta bak vid tanken om målet är att förstå vad spridare och regulator faktiskt ser.

Bränsletemperatur är relevant för:
- pumpkapacitet
- densitet
- tryckdrift
- eventuell värmeuppbyggnad vid låg tanknivå / mycket returflöde

---

# 6. Vevhustryck

## Föreslagen sensor

**Bosch 0 261 230 216**

Mätområde:

```text
0.10–1.15 bar absolute
```

Det smala området är avsiktligt. Vi är främst intresserade av små förändringar runt atmosfärstryck, inte av att kunna mäta ett redan havererat vevhusventilationssystem.

## Placering

Gör en separat liten tryckport i ventilkåpan.

```text
Ventilkåpa
   |
   +---- grov vevhusventilation --> catchcan --> turbo inlet
   |
   +---- liten tryckport --> slang --> Bosch 0 261 230 216
```

Rekommendation:
- separat port hellre än T-koppling långt ute i ventilationsslangen
- 4–6 mm slang är tillräckligt
- placera sensorn högre än tryckuttaget
- undvik en port där kam/kedja kastar olja direkt mot öppningen
- fjärrmontera sensorn för mindre vibration och mindre oljekontakt

Beräknad kanal:

```text
Crankcase Gauge = Crankcase Absolute - BARO
```

Det viktiga blir främst trenden mot cylinderlast/BMEP.

Exempel:

```text
BMEP 30 bar -> +2 kPa
BMEP 33 bar -> +3 kPa
BMEP 35 bar -> +4 kPa
BMEP 37 bar -> +9 kPa
```

En tydlig knäpunkt är viktigare än ett universellt "farligt" absolutvärde.

---

# 7. Kylsystemtryck

## Föreslagen sensor

Honeywell MIP, cirka **4 bar absolute**.

MIP är media-isolerad. "Sealed gauge" beskriver tryckreferensen, inte om givaren är vätsketät.

Beräknad kanal:

```text
Coolant Gauge Pressure = Coolant Pressure Absolute - BARO
```

Användning:
- normalt temperaturberoende kylsystemtryck
- lastkorrelerade tryckspikar
- tidiga tecken på combustion leakage / head lift

Det viktiga är att separera långsam tryckökning från temperatur mot snabba belastningsrelaterade tryckförändringar.

---

# 8. EMAP

## Föreslagen sensor

Honeywell MIP, cirka **8 bar absolute**.

Placering:
- före turbo
- inte direkt på grenröret
- fjärrmonterad från värmen

Föreslagen anslutning:

```text
Exhaust manifold
      |
  ~0.8–1.0 mm orifice
      |
  300–500 mm CuNi / stainless tube
      |
  1–2 coils / loops
      |
  MIP sensor
```

Syftet med metallröret är främst temperaturavlastning.

Den lilla dysan fungerar tillsammans med rörvolymen som ett pneumatiskt lågpassfilter och minskar dessutom mängden het avgasmassa som når sensorn.

Börja omkring:

```text
0.8–1.0 mm
```

Undvik onödigt små hål eftersom sot och kondens kan ge problem.

Beräknad kanal:

```text
EMAP Ratio = EMAP Absolute / MAP Absolute
```

Exempel:

```text
1.2 : 1   mycket bra
1.5 : 1   rimligt
2.0 : 1   tydlig begränsning / mycket drive pressure
```

Exakta gränser är turbosystemberoende. Det viktiga är hur förhållandet förändras när boost och massflöde höjs.

---

# 9. EGT

## Föreslagen sensor

1 st K-type thermocouple, pre-turbine.

Placera givaren där den samlade avgasmassan från alla sex cylindrar är rimligt representerad.

Singel-EGT används främst som trendparameter, inte som skydd mot en enskild varm cylinder.

Logga EGT tillsammans med:
- Lambda
- ignition angle
- EMAP/MAP
- RPM
- BMEP / estimated torque

---

# 10. Bränslepumpsspänning

Pumpens spänning är mycket värdefull tillsammans med Fuel Delta P.

Om bränsletrycket börjar falla vill vi kunna skilja på:
- pumpen är kapacitetsbegränsad
- pumpen får för låg spänning
- relä / kabel / kontakt / jord ger spänningsfall

## Spänningsdelare till MaxxECU

MaxxECU AIN ska inte matas direkt med 12–16 V.

Föreslagen divider:

```text
Pump + ---- 33 kΩ ----+---- AIN
                      |
                     10 kΩ
                      |
                 Sensor GND
```

Rekommenderade motstånd:
- **33 kΩ**
- **10 kΩ**
- helst 0.1–1 % metallfilm
- 0.25 W räcker med mycket stor marginal

Utspänning:

```text
V_AIN = V_PUMP × 10 / (33 + 10)
```

Alltså:

```text
V_AIN = V_PUMP × 0.23256
```

och:

```text
V_PUMP = V_AIN × 4.3
```

Exempel:

| Pump voltage | AIN voltage |
|---:|---:|
| 12.0 V | 2.79 V |
| 13.0 V | 3.02 V |
| 14.0 V | 3.26 V |
| 14.5 V | 3.37 V |
| 16.0 V | 3.72 V |
| 20.0 V | 4.65 V |

33k/10k ger mer marginal än 30k/10k eftersom 20 V fortfarande ligger under 5 V på AIN.

### Filtrering

Lägg gärna:

```text
100 nF
```

mellan AIN och Sensor GND nära ECU:n.

Det ger ett enkelt lågpassfilter tillsammans med spänningsdelarens Thevenin-resistans och hjälper mot elektriskt brus.

## Viktigt om jord

Denna mätning visar pumpens positiva matning relativt ECU:s sensorjord.

För att verifiera installationen bör pumpens jordspänningsfall också kontrolleras med multimeter under hög belastning.

Exempel:

```text
Battery -  -> pump -
```

Om pumpens jord är bra kan ECU-kanalen användas som en mycket bra löpande indikator på pumpens verkliga matningsspänning.

Beräknade kanaler:

```text
Pump Voltage = AIN × 4.3
```

och:

```text
Pump Feed Drop = Battery Voltage - Pump Voltage
```

Observera att den senare även innehåller eventuell skillnad mellan ECU-/sensorjord och pumpjord.

---

# 11. MAP-slang till MaxxECU

Nuvarande slang:

```text
6/4 mm pneumatikslang
ca 1.5 m
```

Det är sannolikt fullt användbart, men kortare är bättre för transientrespons.

Målet är att korta den till ungefär:

```text
0.7–0.8 m
```

Det är en bra kompromiss och bör ge mycket liten praktisk fördröjning.

Kontrollera att:
- 4 mm ID bibehålls
- inga små restriktorer finns i adaptrar
- slangen inte kan knäckas
- vätska/kondens inte kan samlas
- MAP tas från plenum och inte från en enskild runner

Om installationen någon gång byggs om helt är en extern MAP-sensor nära plenum och elektrisk signal tillbaka till ECU den renaste lösningen.

---

# Beräknade kanaler i MaxxECU

## Fuel differential pressure

Absolute fuel pressure sensor:

```text
Fuel Delta P = FuelP - MAP
```

Gauge fuel pressure sensor:

```text
Fuel Delta P = FuelP + BARO - MAP
```

## Crankcase pressure

```text
Crankcase Gauge = CrankcaseAbs - BARO
```

## Coolant pressure

```text
Coolant Gauge = CoolantAbs - BARO
```

## EMAP ratio

```text
EMAP Ratio = EMAP / MAP
```

## Pump voltage

```text
Pump Voltage = PumpVoltageAIN × 4.3
```

## Estimated torque / BMEP

BMEP kan beräknas från uppskattat motormoment.

För M50B25, cirka 2.494 liter:

```text
BMEP [bar] ≈ Torque [Nm] × 0.0504
```

Exempel:

| Torque | BMEP |
|---:|---:|
| 500 Nm | 25.2 bar |
| 600 Nm | 30.2 bar |
| 650 Nm | 32.8 bar |
| 700 Nm | 35.3 bar |
| 750 Nm | 37.8 bar |
| 800 Nm | 40.3 bar |

Momentet kan senare approximeras med en RPM/MAP-tabell som kalibreras mot dynodata.

---

# Mål med instrumenteringen

När motorn trimmas ska belastningen ökas kontrollerat och en parameter i taget.

De viktigaste veto-signalerna är:

```text
Fuel Delta P
Lambda deviation
Knock
Oil pressure
Crankcase pressure
Coolant pressure
EMAP/MAP
EGT
Pump voltage
```

Målet är inte att empiriskt hitta den punkt där något går sönder.

Målet är att hitta den punkt där relationerna börjar förändras:

```text
Crankcase pressure vs BMEP
Coolant pressure vs BMEP
EMAP/MAP vs BMEP
EGT vs BMEP
Fuel Delta P vs fuel temperature / pump voltage
```

En tydlig förändring i lutning eller beteende bör betraktas som en signal att analysera innan cylinderlast eller RPM höjs ytterligare.
