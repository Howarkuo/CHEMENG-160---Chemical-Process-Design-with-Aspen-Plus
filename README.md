# CHEMENG 160 | Chemical Process Design with Aspen Plus

Aspen Plus V14 case studies in vapor–liquid equilibrium, distillation, stoichiometric reactor modeling, and reactor recycle. The projects use stream and component balances to check whether each design meets its separation targets.

## Project overview

| Case | Aspen blocks or method | Main result |
| --- | --- | --- |
| [Ternary flash separation](#ternary-flash-separation) | Two `FLASH2` blocks and a cooler | The second condensate reaches 30.10 wt% ethanol and still contains 1.30 wt% methanol. |
| [Glucose fermentation](#glucose-fermentation-q1) | `RSTOIC` and downstream separation | The troubleshooting example identifies incomplete reactor setup, a missing CO₂ databank link, and an incorrect reaction coefficient. |
| [Ethanol–water distillation](#ethanolwater-distillation-q2) | `RADFRAC`, NRTL | The Aspen run gives 88.0061 wt% ethanol in the distillate and 3.96683 wt% in the bottoms. |
| [Ethanol dehydration](#ethanol-dehydration-q3) | Rectification and 3 Å molecular sieves | The sieve removes residual water beyond the atmospheric azeotrope. |
| [Suspended solids recovery](#suspended-solids-recovery-q4) | Crossflow microfiltration | Water passes through the membrane while mineral fines concentrate in the retentate. |
| [Isobutene production and recycle](#isobutene-production-and-recycle-q5) | `RPLUG`, `SEP2`, `FSPLIT`, `SEP`, `FLASH2` | The recycle run converged. The downstream purifier comparison is pending. |

## Aspen Plus workflow
<img width="2431" height="1305" alt="image" src="https://github.com/user-attachments/assets/7bcdff85-43f5-42bd-b08c-36f50314559e" />


1. In **Properties → Components → Specifications**, add the case components and confirm their databank matches.
2. In **Properties → Methods → Specifications**, select the property method. Use **NRTL** for the alcohol–water and fermentation examples. Keep the original property method and reaction set for the Q5 course template.
3. In **Simulation**, place the blocks, connect material streams, and enter feed conditions and block specifications.
4. Use **Home → Next** or **Check Status** to locate incomplete inputs, then run the flowsheet.
5. Review **Blocks → Results**, **Stream Results**, and **Results Summary → Streams**. Confirm the flow and composition bases before comparing results.

### Common Unit Operations

| Category | Unit operation | Application |
|---|---|---|
| Mixers/Splitters | MIXER | Combines multiple streams |
| Mixers/Splitters | FSPLIT | Splits a stream according to specified fractions |
| Separators | FLASH2 | Separates one vapor phase and one liquid phase using equilibrium |
| Separators | FLASH3 | Separates one vapor phase and two liquid phases |
| Separators | DECANTER | Separates two immiscible liquid phases |
| Separators | SEP | Separates components using specified split fractions |
| Columns | RADFRAC | Rigorous multistage distillation |
| Exchangers | HEATER | Heats or cools a process stream |
| Pressure Changers | PUMP / COMPR | Increases pressure |

## Ternary flash separation

### Objective and operating conditions

<img width="2314" height="1764" alt="image" src="https://github.com/user-attachments/assets/1bd26ae4-7042-4c3a-8226-58cebc8f8cd6" />
<img width="1868" height="972" alt="image" src="https://github.com/user-attachments/assets/e378c8db-6c43-4d4f-9102-2cbd7dc140fa" />

Test whether two equilibrium flashes can recover a liquid containing **more than 30 wt% ethanol** and **less than 0.01 wt% methanol**. These are hypothetical classroom criteria.

Feed **52 kmol/h (999.870 kg/h)** of **89.5 wt% water, 10.0 wt% ethanol, and 0.5 wt% methanol** to the first flash. Use NRTL and **1 atm** throughout. Flash the feed at **95 °C**, cool its vapor to **84 °C**, and separate the resulting vapor and liquid in a second flash. The first liquid leaves directly. The second liquid is condensed from the first vapor.

[Initial flowsheet](https://hackmd.io/_uploads/HyNKFVFnfg.png) · [Initial results](https://hackmd.io/_uploads/rJkesVFnGl.png) · [First flash flow results](https://hackmd.io/_uploads/B16k-HFnzl.png) · [First flash composition](https://hackmd.io/_uploads/BkuPZSFnGl.png) · [Two-flash flowsheet with cooler](https://hackmd.io/_uploads/HJTJbIt3Me.png)

### Stream results
<img width="1315" height="1396" alt="image" src="https://github.com/user-attachments/assets/cbe7b38e-495f-4d94-9f72-00cb297ae8f5" />
<img width="1299" height="1545" alt="image" src="https://github.com/user-attachments/assets/a2c94e6b-0962-4b3c-a91a-580f330ca61c" />

| Stream | Temperature (°C) | Molar flow (kmol/h) | Mass flow (kg/h) | Water (wt%) | Ethanol (wt%) | Methanol (wt%) |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Original feed | — | 52.000 | 999.870 | 89.500 | 10.000 | 0.5000 |
| Liquid 1 | 95 | 44.0725 | 817.745 | 95.146 | 4.566 | 0.2876 |
| Vapor 1 | 95 | 7.92754 | 182.125 | 64.148 | 34.398 | 1.4535 |
| Liquid 2 | 84 | 7.30865 | 162.357 | 68.59 | 30.10 | 1.30 |
| Vapor 2 | 84 | 0.61889 | 19.768 | 27.65 | 69.67 | 2.68 |

The first flash vaporizes **15.25 mol%** of its feed. The second flash condenses **92.19 mol%** of the incoming first-stage vapor. Overall mass balances close to the reported precision: $817.745 + 182.125 = 999.870$ and $162.357 + 19.768 = 182.125$ kg/h.

### Vapor–liquid equilibrium, purity, and recovery

Use the vapor and liquid **mole fractions** to calculate equilibrium ratios. Convert to **mass fractions** before comparing with weight-based specifications.

$$
K_i=\frac{y_i}{x_i},\qquad
\alpha_{i,j}=\frac{K_i}{K_j},\qquad
w_i=\frac{x_i MW_i}{\sum_j x_j MW_j}
$$

| Component | Liquid 2 mole fraction | Vapor 2 mole fraction | Second-flash $K_i$ |
| --- | ---: | ---: | ---: |
| Water | 0.845800 | 0.490229 | 0.580 |
| Ethanol | 0.145162 | 0.483011 | 3.327 |
| Methanol | 0.00903781 | 0.0267595 | 2.961 |

At 84 °C, the ethanol-to-methanol relative volatility is about **1.12**. Both alcohols favor the vapor and their equilibrium ratios are close, so a single stage provides limited selectivity between them.

$$
\text{Purity}_i=\frac{\dot m_{i,\mathrm{product}}}{\dot m_{\mathrm{product}}}\times100\%,\qquad
\text{Recovery}_i=\frac{\dot m_{i,\mathrm{product}}}{\dot m_{i,\mathrm{feed}}}\times100\%
$$

| Selected stream | Reference feed | Ethanol recovery | Methanol recovery |
| --- | --- | ---: | ---: |
| Liquid 1 | Original feed | 37.34% | 47.05% |
| Vapor 1 | Original feed | 62.66% | 52.95% |
| Liquid 2 | Original feed | 48.9% | 42.3% |
| Liquid 2 | Second-flash feed | 78.0% | 80.0% |

For example, Liquid 2 contains approximately $162.357(0.30104)=48.88$ kg/h of ethanol. That is **48.9%** of the **99.987 kg/h** entering the original flowsheet. The condenser step meets the ethanol criterion in Liquid 2, but its methanol concentration is about **130 times** the classroom limit. Neither liquid meets both targets.

## Glucose fermentation (Q1)
<img width="2314" height="952" alt="image" src="https://github.com/user-attachments/assets/3e347039-8e11-426e-aa25-019fcf2a2b7a" />

Model glucose conversion in an `RSTOIC` block called `FERMENT`. Specify the reaction and the fraction of glucose converted. A follow-on `SEP` or `RADFRAC` block can represent downstream separation, depending on the exercise.

$$
\mathrm{C_6H_{12}O_6 \rightarrow 2C_2H_5OH + 2CO_2}
$$

| Location | Issue in the exercise | Fix |
| --- | --- | --- |
| **Blocks → FERMENT → Setup** | Red setup indicator | Complete the required reactor input. The main flowsheet identifies the block but does not show the specific missing field. |
| **Properties → Components → Specifications** | `CO2` component lacks databank details | Link the component to carbon dioxide, CAS **124-38-9**, so Aspen can retrieve its molecular weight and formation enthalpy. |
| **FERMENT → Reactions** | CO₂ entered with a coefficient of **+1** | Use **−1 glucose, +2 ethanol, +2 CO₂**. The glucose conversion specification of **0.50** is independent of the stoichiometric coefficients. |

At the correct stoichiometry, $180.16\approx 2(46.07)+2(44.01)$ g per mol of glucose reacted. Entering only one mole of CO₂ leaves **44.01 g/mol** unaccounted for.

[Illustrative fermentation flowsheet](https://hackmd.io/_uploads/Sy-iJsMaGl.png)

## Ethanol–water distillation (Q2)

### RadFrac inputs

| Input | Setting |
| --- | --- |
| Components and property method | Ethanol, water, NRTL |
| Feed | 5,000 kg/h, 40 °C, 1 atm, 0.30 ethanol and 0.70 water on a **mass-fraction basis** |
| Column | 10 equilibrium stages, total condenser, kettle reboiler |
| Feed location | **Stage 8, Above-Stage**, interpreting “7th tray” as the seventh physical tray. Aspen counts the condenser as stage 1. |
| Pressure | 1 atm throughout, zero stage pressure drop |
| Operating specifications | 39.9 kmol/h distillate, molar reflux ratio 1.0 |
| Products | Liquid distillate from stage 1 and liquid bottoms from stage 10 |
| Condenser outlet | Zero degrees of subcooling, saturated liquid |

### Aspen results

| Stream | Temperature (°C) | Molar flow (kmol/h) | Mass flow (kg/h) | Ethanol (wt%) | Molar vapor fraction |
| --- | ---: | ---: | ---: | ---: | ---: |
| `FEED` | 40.0000 | 226.839 | 5,000.00 | 30.0000 | 0 |
| `DIST` | 78.4372 | 39.900 | 1,548.87 | 88.0061 | 0 |
| `BOTTOMS` | 95.7566 | 186.939 | 3,451.13 | 3.96683 | 0 |

| Equipment | Operating temperature (°C) | Aspen heat duty (cal/s) | Heat duty (kW) |
| --- | ---: | ---: | ---: |
| Total condenser | 78.4372 | −210,626 | −881.26 |
| Kettle reboiler | 95.7566 | +280,968 | +1,175.57 |

The distillate meets the **88 wt%** target at **39.9 kmol/h**, and the bottoms meet the **5 wt% maximum**. The condenser removes heat and the reboiler adds heat. Use the **Heat duty** rows in the block results.

A useful input check is $1500/46.06844 + 3500/18.01528 \approx 226.84$ kmol/h. Entering **0.30 on a mole basis** would give about **189.17 kmol/h** at the same total mass flow.

![RadFrac condenser and reboiler results](https://hackmd.io/_uploads/SJrKcLA0zx.png)

![RadFrac feed and product stream results](https://hackmd.io/_uploads/BJatjUCCGx.png)

## Ethanol dehydration (Q3)

At 1 atm, a **90 wt% ethanol** feed can be concentrated by rectification toward the **approximately 95.6 wt% ethanol** azeotrope. At the azeotrope, the vapor and liquid have the same composition. Further drying can use **3 Å molecular sieves**, **extractive distillation** with an entrainer such as ethylene glycol, or a **water-selective membrane**.

**Selected approach.** Rectify the feed, then send the ethanol-rich vapor to 3 Å zeolite beds. Water enters the pores and adsorbs while ethanol passes through. Alternating beds allow one to dry the feed while the other regenerates under vacuum. [Fixed-bed dehydration study](https://doi.org/10.1002/jctb.280390207) · [Industrial pressure-swing process](https://www.vogelbusch-biocommodities.com/assets/2-ProcessUnits/Brochures/VBC-Dehydration-Process-EN-WEB.pdf)

| Factor | Advantage | Trade-off |
| --- | --- | --- |
| Cost | No liquid entrainer to recover | Requires adsorbent, vessels, switching valves, and vacuum equipment |
| Energy | Can integrate with hot vapor from rectification | Liquid feed must be vaporized and superheated. Regeneration consumes energy and dry purge vapor. |
| Feasibility | Alternating beds support continuous drying | Preconcentration lowers the water load and reduces regeneration demand. |

## Suspended solids recovery (Q4)

For slowly settling mineral fines, use **crossflow microfiltration**. Pressure drives water through the membrane while the solids concentrate in the retentate. Tangential flow helps limit solids buildup, and downstream dewatering can recover the minerals. **Ultrafiltration** is suitable for smaller colloids. **Centrifugation** can be considered when the particles are sufficiently dense, and **coagulation–flocculation followed by filtration** can improve capture if additives are acceptable in the recovered solids.

| Factor | Advantage | Trade-off |
| --- | --- | --- |
| Separation | Low-turbidity permeate and a concentrated mineral stream | Capture depends on pore size. Dissolved contaminants may require additional treatment. |
| Energy | Relatively low transmembrane pressure | Crossflow circulation consumes pumping energy as solids concentration rises. |
| Maintenance | Backwashing and cleaning can restore flux | Fouling, abrasion, membrane replacement, and retentate dewatering add operating costs. |

## Isobutene production and recycle (Q5)

### Process and Aspen setup

Use the Aspen template in **bCourses → Assignments**. Fresh isobutane enters at **125 kmol/h, 25 °C, and 5 bar**. The catalytic vapor-phase PFR operates at **600 °C and 3 bar** with the template's reversible LHHW kinetics.

$$
\mathrm{C_4H_{10} \rightleftharpoons C_4H_8 + H_2}
$$

`C1` cools the PFR outlet to **85 °C**. The template's `SEP1` removes hydrogen, and the remaining C₄ stream, `REST`, goes to `SPLIT`.

~~~mermaid
flowchart TD
    F["Fresh feed heater H1"] --> M["MIX"]
    M --> R["PFR"]
    R --> C["Cooler C1"]
    C --> S["Hydrogen separator SEP1"]
    S --> H["H₂ product"]
    S -->|REST| X["SPLIT"]
    X -->|80% RECYCLE| RH["Recycle heater RH"]
    RH -->|REC-HOT| M
    X -->|20% PURIF-IN| P["Purifier"]
~~~

1. Reconnect the destination of **`REST`** to the `FSPLIT` material inlet. Set the **`RECYCLE` split fraction to 0.80** and leave `PURIF-IN` blank.
2. Send `RECYCLE` to `RH`. Set its outlet to **600 °C and 3 bar**. The observed cold recycle is already at 3 bar.
3. Reconnect `H1OUT` to `MIX`. Connect `REC-HOT` from `RH` to the mixer and `PFR-IN` from the mixer outlet to the PFR. Verify that the combined reactor feed is vapor at **600 °C and 3 bar**.
4. Run the recycle case. Compare the **From**, **To**, and **Mole Flows** rows. Use **Reconnect Destination** for an incorrect receiving block and **Reconnect Source** for an incorrect supplying block.

[Original template flowsheet](https://hackmd.io/_uploads/S1AXgPA0zl.png) · [Splitter setup](https://hackmd.io/_uploads/r16GYPCRzx.png)

### Recycle balance

| C₄ stream | Open loop observed (kmol/h) | Closed loop observed (kmol/h) | Closed-loop balance (kmol/h) |
| --- | ---: | ---: | ---: |
| `REST` | 125.000 | 625.012 | 625 |
| `RECYCLE` | 100.000 | 500.010 | 500 |
| `PURIF-IN` | 25.000 | 125.002 | 125 |

The reaction conserves total C₄ moles, and hydrogen leaves through a separate outlet. At steady state, the C₄ leaving through `PURIF-IN` equals the fresh C₄ feed, so $0.20\dot n_{\mathrm{REST}}=125$ kmol/h. In the converged case, `PURIF-IN` is all vapor at **85 °C and 3 bar**.

![Closed-loop splitter stream results](https://hackmd.io/_uploads/H1yEJuRAMe.png)

### Purifier comparison

Use a copy of the converged case for each purifier so both receive the full `PURIF-IN` stream. In each run, use **Mole Fractions** for isobutene purity and **Mole Flows** to calculate recovery.

| Case | Setup | Product and interpretation |
| --- | --- | --- |
| `FLASH2` | First test at 85 °C and 3 bar. For a two-phase comparison, specify **3 bar and molar vapor fraction 0.50** in place of temperature. | At the original feed conditions there is no liquid phase to collect. In the two-phase trial, compare the isobutene-enriched `FLASH-LIQ` with the vapor outlet. |
| `SEP` membrane model | In **Specifications → PERMEATE → MIXED**, enter split fractions of **0.95 for `C4H8-5`** (isobutene), **0.05 for `C4H10`** (isobutane), and **0 for `H2`**. Leave `RETENTATE` unspecified. | The splits are illustrative inputs. The isobutene recovery is fixed at 95% by the specified split. Permeate purity depends on the feed composition and the isobutane split. |

$$
\text{Isobutene recovery}
=\frac{\dot n_{\mathrm{isobutene,product}}}{\dot n_{\mathrm{isobutene,PURIF-IN}}}\times100\%
$$

**Selected concept.** A single flash relies on a small volatility difference between isobutane and isobutene. An olefin-selective membrane can exploit isobutene's double bond, for example through reversible binding to a silver-ion carrier. The `SEP` result represents the specified splits. Demonstrating real membrane performance requires mixed-gas permeation data. [Isobutane phase data](https://webbook.nist.gov/cgi/cbook.cgi?ID=C75285&Type=TBOIL) · [Isobutene phase data](https://webbook.nist.gov/cgi/cbook.cgi?ID=C115117&Mask=4) · [Facilitated-transport membrane study](https://doi.org/10.1016/S0376-7388(97)00231-7)

**Completion status.** The notes document the converged recycle and the purifier setup. Final `SEP` and `FLASH2` results are pending. Record the inlet and outlet component flows, isobutene purity and recovery, and the input and stream-results screenshots for both units when the runs are available. [Additional Q5 screenshots](https://hackmd.io/_uploads/SJME-_00zg.png) · [Aspen setup screenshot](https://hackmd.io/_uploads/BymvWdRCfe.png)

## References

- [Aspen Plus Getting Started: Building and Running a Process Model](https://zakrzewska.zut.edu.pl/AspenPlusProcModelV8_4-Start.pdf)
- [Aspen Plus User Guide: Flowsheet Connections and Unit Operation Models](https://user.eng.umd.edu/~nsw/chbe446/AspenPlusV10UserGuide1.pdf)
