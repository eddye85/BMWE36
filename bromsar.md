# Porsche 996 Brake Conversion for BMW E36
Teknisk sammanställning över val av ok, skivor, kolvareor, bias-beräkningar, huvudcylinderval och SFRO-beräkningar.

---

## 1. Valda bromsdelar

### Fram (slutlig setup)

**Porsche 996 C2/C4 4-kolvs monoblock (Brembo)**  
Artikelnummer:
- 996.351.425 (vänster)
- 996.351.426 (höger)

Kolvdiametrar:
- 36 mm
- 40 mm

Kolvareor (per kolv):
- 36 mm → π·18² ≈ **1018 mm²**
- 40 mm → π·20² ≈ **1257 mm²**

Per sida (2 kolvar):
- A_f,sida = 1018 + 1257 ≈ **2275 mm²**

Per ok (båda sidor):
- A_f,tot = 2 · 2275 ≈ **4549 mm²**

**Framskiva (slutligt mål):**
- 345×28 mm (E46 M3 CSL)
- Effektiv radie r_f,eff ≈ **145 mm**

**Framskiva (temporärt):**
- 325×25 mm (E46 330Ci)
- Effektiv radie r_f,eff,temporär ≈ **140 mm**

---

### Bak (slutlig setup)

**Porsche 996 bakok (Brembo)**  
Artikelnummer:
- 996.352.421 (vänster)
- 996.352.422 (höger)

Kolvdiametrar:
- 28 mm
- 30 mm

Kolvareor (per kolv):
- 28 mm → π·14² ≈ **616 mm²**
- 30 mm → π·15² ≈ **707 mm²**

Per sida (2 kolvar):
- A_r,sida = 616 + 707 ≈ **1323 mm²**

Per ok (båda sidor):
- A_r,tot = 2 · 1323 ≈ **2645 mm²**

**Bakskiva:**
- 320×22 mm (E46 330Ci, drum-in-hat)
- Effektiv radie r_r,eff ≈ **135 mm**

---

## 2. Friktionskoefficienter (belägg)

| Beläggtyp  | Friktionskoefficient μ_pad |
|-----------|----------------------------|
| Gatbelägg | **0,40**                   |
| Racebelägg| **0,50**                   |

Dessa används för att uppskatta verkligt bromsmoment på skivan.

---

## 3. Hydrauliskt “moment” per hjul (för bias)

Vi använder per-sida-arean (en sida av oket) och effektiv radie:

> M ∝ A_sida · r_eff

### Fram (per hjul)

- A_f,sida ≈ 2275 mm²  
- r_f,eff ≈ 145 mm  

M_f = 2275 · 145 ≈ **329 804**

### Bak (per hjul)

- A_r,sida ≈ 1323 mm²  
- r_r,eff ≈ 135 mm  

M_r = 1323 · 135 ≈ **178 552**

(Värdena är i godtyckliga “momentenheter”, används bara för att jämföra fram/bak.)

---

## 4. Brake bias (statisk)

$$
\text{bias}_f = \frac{M_f}{M_f + M_r}
$$

$$
\text{bias}_f = \frac{329804}{329804 + 178552} \approx 0{,}649
$$

### → **Brake bias ≈ 65 % fram / 35 % bak**

- Mer bakbroms än original E36 325i (ca 70/30)
- I nivå med eller något mer bak än E36 M3 / nyare BMW
- Helt rimligt med bra belägg och ABS

---

## 5. Huvudbromscylindrar & pedaltravel

Pedaltravel (för givet linjetryck) är ungefär proportionell mot:

> Travel ∝ A_caliper / A_MC

### MC-areor

**E36 325i original MC 23,81 mm**

- A_MC,23.8 = π·11,905² ≈ **445 mm²**

**E36 M3 MC 25,4 mm**

- A_MC,25.4 = π·12,7² ≈ **507 mm²**

Förhållande:

- A_MC,25.4 / A_MC,23.8 ≈ 507 / 445 ≈ **1,14**

### → M3-huvudcylinder:

- Ca **14 % kortare pedaltravel**
- Ca **14 % högre pedalkraft** för samma linjetryck
- Bättre modulering och “race-känsla”

---

## 6. Samlad broms-spec

| Komponent | Data |
|----------|------|
| Framok   | Porsche 996, 36/40 mm |
| Bakok    | Porsche 996, 28/30 mm |
| Skiva fram | 345×28 mm (E46 CSL), r_eff ≈ 145 mm |
| Skiva bak  | 320×22 mm (E46 330Ci), r_eff ≈ 135 mm |
| Brake bias | ≈ **65 % fram / 35 % bak** |
| MC-val     | 23,8 mm (OEM) eller 25,4 mm (M3) |
| Pedaltravel | ≈ −14 % med 25,4 mm MC |

---

## 7. Antaganden för SFRO-beräkningar

För att visa marginaler i bultar och adapter antar vi:

- Tjänstevikt: **1300 kg**
- Max dimensionerande inbromsning: **1,0 g**
  - a = 9,81 m/s²
- Total bromskraft vid hjulen:
  - F_tot = m·a ≈ 1300 · 9,81 ≈ **12 753 N**
- Hydraulisk bias: 65/35 fram/bak
- Hjulradie (rullradie): **0,31 m**
- Beläggfriktion (gatbelägg): μ_pad = 0,40
- Skivradier:
  - r_f,eff = 0,145 m
  - r_r,eff = 0,135 m

---

## 8. Bromskrafter per axel och hjul (1,0 g)

**Total:**

- F_tot ≈ **12 753 N**

**Framaxel:**

- F_f,axel = 0,649 · 12 753 ≈ **8 274 N**
- Per framhjul: F_f,hjul ≈ **4 137 N**

**Bakaxel:**

- F_r,axel = 0,351 · 12 753 ≈ **4 479 N**
- Per bakhjul: F_r,hjul ≈ **2 240 N**

---

## 9. Nödvändigt bromsmoment per hjul

> T = F_hjul · r_hjul

**Fram:**

- T_f = 4 137 · 0,31 ≈ **1 282 Nm per framhjul**

**Bak:**

- T_r = 2 240 · 0,31 ≈ **694 Nm per bakhjul**

---

## 10. Nödvändigt linjetryck

Vi använder:

> T = μ_pad · P · A_tot · r_eff  

där:
- P = linjetryck (Pa)
- A_tot = total kolvarea per ok (båda sidor), i m²
- r_eff = effektiv skivradie (m)

Areor i m²:

- A_f,tot = 4549 mm² = **4,549·10⁻³ m²**
- A_r,tot = 2645 mm² = **2,645·10⁻³ m²**

**Fram:**

$$
P_f = \frac{T_f}{\mu_{pad} \cdot A_{f,tot} \cdot r_{f,eff}}
$$

$$
P_f \approx \frac{1282}{0{,}40 \cdot 4{,}549\cdot10^{-3} \cdot 0{,}145} \approx 4{,}86\cdot10^6 \,\text{Pa} \approx **48,6 bar**
$$

Bak blir samma (systemet är dimensionerat med samma tryck).

**→ 1,0 g kräver ungefär 50 bar linjetryck.**  
Ett normalt system klarar 80–120+ bar utan problem → god marginal.

---

## 11. Skjuvlast i okbultar / adapterbultar

Antag:

- 2 st M12 12.9-bultar som håller oket (i adapter/spindel)
- Friktionskraft mellan belägg och skiva approx F_disc,f ≈ μ_pad · F_clamp

För P ≈ 48,6 bar:

- F_clamp,f ≈ 22 100 N  
- F_disc,f ≈ 0,40 · 22 100 ≈ **8 840 N**

Vi förenklar och antar:

- F per bult ≈ F_disc,f / 2 ≈ **4 420 N**

**M12 12.9 – skjuvkapacitet**

- Kärnarea M12 ≈ 113 mm² = 113·10⁻⁶ m²  
- R_m (12.9) ≈ 1200 MPa  
- Skjuvhållfasthet τ_y ≈ 0,58·R_m ≈ 696 MPa

Enkelt skjuvplan:

- F_y,single ≈ 696·10⁶ · 113·10⁻⁶ ≈ **78 700 N**

Dubbel skjuv per bult:

- F_y,double ≈ **157 000 N**

Två bultar:

- F_y,tot ≈ **314 000 N**

**Säkerhetsfaktor (skjuv, bult):**

- SF_skjuv,bult = 314 000 / 8 840 ≈ **35**

→ Ca **35× säkerhetsfaktor** i skjuv på okbultarna vid 1,0 g.

---

## 12. Böjspänning i adapter (överslag)

Antag:

- Kraft från ok mot adapter: F_disc,f ≈ **8 840 N**
- Effektivt hävarm: l ≈ **75 mm** = 0,075 m
- Adaptermaterial: Alumec 89 (≈ 7000-serie Al, Rp0,2 ≈ 500 MPa)
- Minsta tvärsnitt i örat: b = **20 mm**, h = **40 mm**

Böjmoment:

- M_adapter = F_disc,f · l ≈ 8 840 · 0,075 ≈ **663 Nm**

Böjmotstånd (rektangel):

- W = b·h² / 6 = 20·40²/6 ≈ 5 333 mm³ = 5,33·10⁻⁶ m³

Spänning:

- σ = M / W ≈ 663 / (5,33·10⁻⁶) ≈ **124 MPa**

Säkerhetsfaktor:

- SF_böj,adapter ≈ 500 / 124 ≈ **4**

→ Cirka **4× säkerhetsfaktor i böjspänning** i adapterörat vid 1,0 g, med antagna mått.

(Ökar du tjocklek/höjd eller kortar hävarmen ökar SF.)

---

## 13. Slutsats (SFRO-perspektiv)

Med antaganden ovan:

- 1,0 g inbromsning kräver bara ≈ 50 bar linjetryck  
- Okbultar (M12 12.9) har ≈ **35×** säkerhetsfaktor i skjuv  
- Adapter i Alumec 89 med tvärsnitt 20×40 mm och 75 mm hävarm har ≈ **4×** säkerhetsfaktor i böj  
- Systemet är väl dimensionerat för hård bankörning med god marginal mot brott

Detta dokument kan användas som tekniskt underlag mot SFRO.  
Uppdatera gärna med faktiska CAD-mått när adaptern är färdigritad.
