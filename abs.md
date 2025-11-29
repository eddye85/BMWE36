# MK60E1 Standalone ABS – BMW E36 Retrofit Guide

Den här guiden sammanfattar allt du behöver för att köra **BMW MK60E1 ABS** i standalone-konfiguration i en BMW E36.  
Den inkluderar rätt komponenter, artikelnummer, sensorer, rekommenderad yaw-sensor, samt varför MK60E1 är ett bättre val än MK60E5 för retrofit-projekt.

---

# ⭐ Varför MK60E1?
MK60E1 är den mest kompatibla, enklaste och mest väldokumenterade ABS-enheten att retrofitta i äldre BMW-chassin som E30/E36/E46.  
Den kommer från 4-cyl E90/E87-modeller och har:

- Intern **input brake pressure sensor**
- Full CAN-broadcast (500 kbps)
- Kompatibilitet med **VR-hjulgivare** (samma typ som E36/E46)
- Möjlighet att flashas med **Continental/Motorsport-firmware**
- Kodningsmöjligheter för hjulbas, vikt, däck, sensorlayout mm.
- Låg kostnad (300–700 kr på skrot)
- Utmärkt prestanda i trackday-bilar (E36, E46, Miata, Lotus osv.)

För E36 ger MK60E1 i praktiken **lika bra eller bättre prestanda** än E46 M3 MK60 (som är dyr och kräver externa sensorer).

---

# ❗ Varför inte MK60E5?
MK60E5 låter bättre på papperet, men är i praktiken ett stort problem vid retrofit.

| Egenskap | MK60E1 | MK60E5 |
|----------|--------|--------|
| Kompatibel med VR-hjulsensorer | ✔️ | ❌ (kräver Hall-kodade sensorer) |
| Intern input pressure sensor | ✔️ | ✔️ |
| Intern output pressure sensors | ❌ | ✔️ |
| Flashbar med Motorsport/Conti firmware | ✔️ | ⚠️ Endast vissa ovanliga varianter |
| Installation i E36 | Enkel | Komplicerad |
| Krav på sensorer | Standard | Specialsensorer (dyra och svåra att hitta) |
| Prestandavinst i E36 | Mycket hög | Marginell (om inte allt annat byggs om) |

De flesta MK60E5-enheter (från E90 6-cyl och E92 M3) är **inte lämpliga** för standalone.  
Den enda “bra” E5-varianten kommer från vissa Z4M – men är extremt sällsynt.

**Därför är MK60E1 rätt val.**

---

# 🔧 MK60E1 – Artikelnummer och identifiering

## **Bosch-nummer (säkraste sättet)**  
Alla MK60E1 börjar med:
0 265 960 0xx


Om Boschnumret har **960** i mitten är det garanterat MK60E1.

## **BMW-nummer (exempel)**
Vanliga E1-pumpar från E90/E87 4-cyl:

- 34 51 6 784 765  
- 34 51 6 784 766  
- 34 51 6 789 936  
- 34 51 6 780 308  
- 34 51 6 789 690  

## **Bilar att leta på**
Dessa har alltid MK60E1:

- **E90 318i / 320i** (4-cyl)  
- **E87 116i / 118i / 120i**  
- Årsmodeller ca 2005–2011

## **Visuell identifiering**
MK60E1 har:
- En enda toppkåpa (ingen sidobulle)
- En 38-pins kontakt
- Kompakt hydraulblock
- Inga externa tryckgivare

---

# 🎯 Rekommenderad Yaw-sensor (fet variant, 6-pin)

Den “feta” sensorn från E90 är den mest beprövade och används i nästan alla MK60-installationer.

## **BMW-nummer**
- 34 52 6 764 018  
- 34 52 6 855 707

## **Bosch-nummer**
- 0 265 005 303  
- 0 265 005 304  
- 0 265 005 305  

## **Funktion**
- Yaw rate  
- Lateral acceleration  
- Fullt kompatibel med MK60E1

## **Pinout (6-pin)**
Pin 1 – Ground
Pin 2 – CAN Low
Pin 3 – CAN High
Pin 4 – +12V
Pin 5 – Not used
Pin 6 – Not used


## **Placering i E36**
- På kardantunneln, centrerat i bilen  
- Pilen pekar framåt  
- Monteras plant  
- Gärna med vibrationsdämpning

---

# 📡 CAN-data som MK60E1 broadcastar

MK60E1 skickar automatiskt ut:

- Individuell hjulhastighet (LF/RF/LR/RR)  
- Input brake pressure  
- ABS intervention states  
- Yaw rate  
- Longitudinal G  
- Lateral G  
- Brake switch state  
- Wheel slip data  
- Wheel speed scaling / tire diameter offsets

---

# 🧰 Installation i BMW E36

För E36 krävs:

- MK60E1 ABS-hydraulenhet  
- E90 6-pin yaw sensor  
- 4 hjulsensorer (E36 original VR fungerar)  
- Brake switch → relay conversion (N/C to N/O)  
- 12V matning (3 separata strömmatningar, 5A + 20A + 30A)  
- CAN High/Low till yaw och OBD  
- 4-kanals bromsledning (E36 original är 3-kanal, så extra linje bak)  

Vill du ha färdig pinout för E36 → MK60E1 kan det enkelt läggas till.

---

# 📦 Nästa steg
När du har en MK60E1 och yaw-sensor kan du:

1. **Installera mekaniskt (bracket + bromslinjer)**  
2. **Bygga wiring loom (jag kan ta fram schema)**  
3. **Testa OBD-anslutning och läsa CAN**  
4. **Flashning (valfritt) med motorsport/Conti firmware**  
5. **Finjustera ABS-parameters via NCS Expert**

---

# 📜 Licens
Fritt att använda, dela och modifiera.  
Perfekt som grund för eget projekt, GitHub-repo eller dokumentation till ditt bygge.
