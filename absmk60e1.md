# MK60E1 Standalone ABS Retrofit – BMW E36

Den här guiden beskriver hur man installerar en **BMW MK60E1 ABS-modul** från E90/E87 (fyrcylindrig modell) i en BMW E36.  
Versionen MK60E1 är den modernaste varianten som fungerar med **vanliga VR-hjulgivare**, har **inbyggd trycksensor**, kan **kodas om**, och går att köra helt standalone.

Detta är dokumenterat, testat och beprövat i många plattformar: E36, E30, Miata, Lotus Elise, K-swaps, racebilar och Time Attack-bilar.

---

# ⭐ Varför MK60E1?

## Fördelar mot original E36 ABS:
- 4-kanals aktiv modulering  
- mycket snabbare ventiltider  
- bättre kontroll under trail braking  
- mindre risk för "ice mode"  
- data via CAN (hjulhastigheter, tryck mm)  
- möjlig kodning av parametrar (hjulbas, vikt, tröghetsmoment, däck mm)  
- fungerar med E36 original hjulgivare (VR, tvåtråd)

## Varför E1 över E5?
| Egenskap | MK60E1 | MK60E5 |
|---------|--------|--------|
| Hjulsensorer | VR, vanlig | Digital enkoder (incompatible) |
| Intern trycksensor | Ja | Ja + outputs |
| Kodningsmöjlighet | Ja | Ja |
| Kostnad | Låg | Låg |
| Kompatibilitet med E36 | **Mycket hög** | Låg |
| Flashbar med Conti Race | Ja | Endast Z4M-spec E5 |

**Slutsats: MK60E1 är den rätta versionen för E36.**

---

# 🔍 Rätt hårdvara

## Rekommenderad pump (bekräftad E1)
Du har hittat en pump med följande nummer:

- **6 787 837** (ECU)
- **6 791 521** (hydraulblock)

→ Dessa är 100 procent **MK60E1** från en **E90 318i/320i** (N43-motor).  
→ Den fungerar perfekt för standalone.

## Full lista med E1-kompatibla donatorbilar:
- BMW E90 316i/318i/320i (fyrcyl)
- BMW E91 318i/320i (fyrcyl)
- BMW E87 116i/118i/120i (fyrcyl)
- BMW E81 116i/118i (fyrcyl)

---

# 🔌 Kontaktstycke (Connector)

## ABS ECU 47-pin kontakt (Bosch MK60E1)
- **BMW / TE Connectivity 1718760-1**  
OBS: Den säljs ibland som ATE MK60 / “47-pin ABS connector”.

## Var köper man?
- **Tulay Wirewerks** (paket med pins):  
  https://www.tulayswirewerks.com  
  (sök: *Teves MK60 ABS Connector Kit*)

- **TE Connectivity återförsäljare:**  
  Digi-Key: Sök på *1718760-1*  
  Mouser: Sök på *1718760-1*

Du behöver:
- 1× 47-pin housing  
- 47× 0.5–1.0 mm crimp terminals (female)  
- Lock wedge (secondary lock), TE-nr 1718760-2 (valfritt)

---

# 🧩 Pinout + wiring guide (E36 → MK60E1)

## E36 original ABS-kablar som kan återanvändas
Du återanvänder:
- alla fyra hjulhastighetssensorernas signalkablar  
- +12V matning  
- tändningsmatning  
- jord  
- bromsljuskontaktens logik (men via relä)

Du *måste lägga till*:
- CAN High / Low (PT-CAN) för diagnos och loggning  
- CAN High / Low (F-CAN) till yaw sensor  
- 12V matning till yaw sensor  
- ev. separat säkringsbox (rekommenderas)  

---

# 📌 MK60E1 Pinout (förenklad, bara det du behöver)

| Pin | Funktion | Signal från E36 |
|-----|----------|----------------|
| 1 | +12V (40A) | ABS huvudmatning |
| 4 | Brake switch (ground = off, open = pedal pressed) | via relä |
| 11 | F-CAN High | till yaw sensor |
| 16 | Chassi ground | jordpunkt |
| 17 | +12V IGN (5A) | E36 ABS tändning |
| 26 | F-CAN Low | till yaw sensor |
| 27 | Ground yaw sensor | jord |
| 29 | +12V IGN (5A) | spliced med 17 |
| 30 | PT-CAN High | diagnos/loggning |
| 32 | +12V (30A) | ABS huvudmatning |
| 33 | FR wheel speed signal | E36 FR signal |
| 34 | FR wheel speed power | E36 FR + |
| 36 | RL wheel speed power | E36 RL + |
| 37 | RL wheel speed signal | E36 RL signal |
| 39 | +12V yaw sensor | matning |
| 42 | RR wheel speed signal | E36 RR signal |
| 43 | RR wheel speed power | E36 RR + |
| 45 | FL wheel speed power | E36 FL + |
| 46 | FL wheel speed signal | E36 FL signal |
| 47 | Ground | jordpunkt |

---

# ⛓ Bromsrörschema för MK60E1 i E36

MK60E1 har **sex portar**:

### Portar mot huvudcylindern:
- **Vorne** (front) – M12x1.0 bubble
- **Hinten** (rear) – M12x1.0 bubble

### Utgångar:
- LF – M12x1.0
- RF – M10x1.0
- LR – M12x1.0
- RR – M10x1.0

E36 är original 3-kanal → du måste dra **ett extra rör till höger bak**.

Rekommenderat:
- Nickelpansar (NiCopp) 3/16"
- AN-3 rostfritt till flexdelar
- AN3 → M10/M12 adapters vid ABS-pumpen

---

# 📡 CAN DBC (för AIM, MaxxECU, Motec, logger)

Minimal DBC baserat på MK60E1/E5 E90-protokoll:

```dbc
VERSION "MK60E1 BMW ABS"
NS_ :
BS_ :
BU_: MK60E1

BO_ 0x1A6 ABS_WHEEL_SPEEDS: 8 MK60E1
 SG_ FL_wheel_speed : 0|16@1+ (0.01,0) [0|655] "km/h" Receiver
 SG_ FR_wheel_speed : 16|16@1+ (0.01,0) [0|655] "km/h" Receiver
 SG_ RL_wheel_speed : 32|16@1+ (0.01,0) [0|655] "km/h" Receiver
 SG_ RR_wheel_speed : 48|16@1+ (0.01,0) [0|655] "km/h" Receiver

BO_ 0x1A5 ABS_STATUS: 8 MK60E1
 SG_ brake_pressure_input : 0|16@1+ (0.1,0) [0|200] "bar" Receiver
 SG_ abs_active : 32|1@1+ (1,0) [0|1] "" Receiver
 SG_ failure : 33|1@1+ (1,0) [0|1] "" Receiver

BO_ 0x1AC YAW_G_LAT_LONG: 8 MK60E1
 SG_ yaw_rate : 0|16@1- (0.01,0) [-200|200] "deg/s" Receiver
 SG_ lat_acc : 16|16@1+ (0.01,0) [-6|6] "g" Receiver
 SG_ long_acc : 32|16@1+ (0.01,0) [-6|6] "g" Receiver
