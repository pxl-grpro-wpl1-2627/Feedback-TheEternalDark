# Reflectie — PE: C# Console (The Eternal Dark, iteratie 2)

**Student:** ArchmageEternal
**Opgave:** iteratie 2 — de hond, de engelentraan en het drakenoog
**Vergeleken met:** de lectoroplossing in deze repository

---

## 1. Wat ik heb ingeleverd

Ik heb per ticketgroep één commit gemaakt, in deze volgorde:

| Commit | Tickets | Bestanden |
| --- | --- | --- |
| `DOG-01..03` | hond | `Biome.cs`, `Player.cs` |
| `TEAR-01..04` | engelentraan | `Player.cs` |
| `EYE-01..03` | drakenoog | `Player.cs` |
| `FIND-01..03` | voorwerpen vinden | `World.cs` |

`dotnet build` geeft **0 errors en 0 warnings**. De gegeven bestanden
(`Program.cs`, `Display.cs`, `WorldArt.cs`) heb ik niet aangeraakt: een diff met
de lectorversie geeft daar nul verschillende regels.

---

## 2. Wat goed ging

- **Alle 13 tickets zijn functioneel correct.** De drie `Has...`-properties staan
  op `Player`, staan in de constructor op `false`, en `TryFindSpecialItem()`
  zet de juiste property en geeft `null` terug als er niets gevonden is.
- **`EffectiveSunCost` is op alle drie de plaatsen gebruikt** in `BuildSun()`:
  bij de controle, bij het aftrekken én in de foutboodschap. Dat is precies wat
  TEAR-04 vraagt, en dat is makkelijk om half te doen.
- **`Math.Max(..., 0)`** zit er, dus de kostprijs kan nooit negatief worden.
- **Geen publieke velden.** Alles is een property, zoals in de opdracht staat.
- **De kansberekening klopt.** `rnd.Next(1, ItemFindChance + 1) == 1` geeft
  effectief 1 op 8, hetzelfde als de lectoroplossing.
- **De drie voorwerpbiomen zijn gegarandeerd verschillend**, omdat ik elke
  gekozen letter uit de pool verwijder met `Remove(pick, 1)`.
- **Ik heb per ticket gecommit.** Daardoor kon ik elke feature apart terugdraaien
  als iets stukging.

---

## 3. Wat minder goed ging

| # | Probleem | Waar | Wat de lector doet |
| --- | --- | --- | --- |
| 1 | **Dubbele code**: de logica om distinct letters te kiezen staat twee keer (constructor én `GenerateBiomeSeed`). | `World.cs` | Eén private `PickDistinctLetters(string pool, int count)`, voor beide gevallen gebruikt. |
| 2 | **Verouderde commentaar**: de bestandskop zegt nog "Standard solution for iteration 1 (WORLD-01 .. WORLD-04)", terwijl er iteratie-2-code onder staat. | `World.cs`, `Player.cs`, `Biome.cs` | Commentaar beschrijft de huidige code, inclusief waarom. |
| 3 | **Taalfout in een spelbericht**: `Name + " You are already at camp."` → "Bob You are already at camp." Het werkwoord ontbreekt. | `Player.ReturnToCamp()` | `Name + " is already at camp."` |
| 4 | **Spelfouten in spelteksten**: "alredy shinning", "You need go back to the camp", "thats finally pretty warm right here". | `Player.BuildSun()` | Neutrale, correcte zinnen. |
| 5 | **Dode code**: in de branch waarin de speler al op kamp staat, zet ik nog `Location = 0;` terwijl dat al zo is. | `Player.ReturnToCamp()` | Die regel staat er niet. |
| 6 | **Random-stijl**: `rnd.Next(1, ItemFindChance + 1) == 1` is correct, maar minder leesbaar dan de 0-gebaseerde variant. | `World.TryFindSpecialItem()` | `rnd.Next(0, ItemFindChance) == 0` |
| 7 | **Rommel in de file**: een lege regel met alleen spaties en een commentaarblok dat los staat van de methode. | `World.cs` | Opgeruimde opmaak. |

Verder is het verschil in omvang van de wereld een bewuste keuze, geen fout: de
lector gebruikt 7 biomen (`bcefghi`, inclusief de easter eggs) en ik hou het bij
de 4 die de opdracht voorschrijft (`bcef`). Dat is functioneel even correct,
maar de lectoroplossing is wel generieker opgeschreven, waardoor die extra
biomen bijna gratis meekomen.

---

## 4. Waarom ging het minder goed?

- **Ik stopte bij "het werkt".** Per ticket testte ik of het spel deed wat het
  moest doen, en dan ging ik verder naar het volgende ticket. Een review-pass
  over de hele file op het einde heb ik niet gedaan.
- **Copy-paste in plaats van extraheren.** Toen ik in de constructor al
  "kies drie verschillende letters" had geschreven, heb ik dat bij
  `GenerateBiomeSeed` opnieuw getypt in plaats van er een methode van te maken.
  Op dat moment was dat sneller, maar het kost nu onderhoud.
- **Strings behandelde ik als code, niet als tekst.** Ze compileren allemaal, dus
  ik controleerde ze niet op grammatica. Dat is jammer, want de speler leest die
  zinnen als enige van mijn werk.
- **Commentaar zag ik als decoratie van de Startcode.** Ik ben vergeten om de
  koppen bij te werken toen de code veranderde.
- **Geen tests.** Ik heb niets geautomatiseerd, dus ik moest vertrouwen op
  manueel spelen. Daardoor zijn dingen als `Location = 0` bij "al op kamp" en de
  taalfout er doorgeglipt.

---

## 5. Wat ik meeneem naar de volgende keer

1. **Regel voor mezelf:** schrijf ik dezelfde 5+ regels twee keer, dan wordt het
   een methode met parameters. In dit geval: `PickDistinctLetters(pool, count)`.
2. **Verplichte review-pass vóór de laatste commit:** de volledige diff lezen en
   specifiek checken op (a) commentaar dat niet meer klopt, (b) dode regels,
   (c) spelteksten.
3. **Spelteksten hardop lezen.** Als een zin raar klinkt, is hij fout.
4. **Eén vaste stijl voor random** (0-gebaseerd) en de kans in commentaar zetten.
5. **Kleine asserts toevoegen**, ook als de opdracht er niet om vraagt,
   bijvoorbeeld: `EffectiveSunCost` is nooit negatief, en
   `DogBiome != AngelTearBiome`.

---

## 6. Wat ik al goed beheers

- **Properties en encapsulatie** — geen publieke velden, `get; set;` waar nodig.
- **Constructors** en het correct initialiseren van alle velden.
- **`const` vs. gewone velden**, en `private` vs. `public` constanten.
- **Methodes met parameters en returnwaarden**, en private hulpmethodes
  (`EffectiveSunCost`).
- **`Math.Max` / `Math.Abs`** en integer-deling.
- **`Random`** en kansberekening.
- **Stringmanipulatie**: indexering, `IndexOf`, `Substring`, `Remove`.
- **If/else met vroege returns** in plaats van diepe nesting.

---

## 7. Wat ik nog niet onder de knie heb

- **Refactoren en methodes extraheren (DRY).** Dit is mijn grootste zwakte in
  deze PE: ik schrijf liever opnieuw dan dat ik generaliseer.
- **Code review van mijn eigen werk.** Commentaar up-to-date houden en dode code
  verwijderen gaat bij mij systematisch mis.
- **Nette, consistente output.** Grammatica en toon in spelberichten.
- **Kleinschalig testen.** Ik leun te veel op manueel spelen in plaats van op
  asserts of unit tests.
- **Naamgeving van hulpmethodes en constanten** die ook voor een tweede gebruik
  werkt, niet alleen voor het geval waarvoor ik ze schrijf.

---

## 8. Concreet plan

1. `World.cs` herschrijven met één `PickDistinctLetters(pool, count)` en
   opnieuw builden.
2. De comments in alle drie de bestanden bijwerken naar iteratie 2.
3. Alle spelberichten herlezen en de taalfouten eruit halen.
4. Twee oefeningen maken rond methodes extraheren (een lang blok opsplitsen in
   kleine methodes met parameters).
5. Bij de volgende PE: eerst ticketlijst, dan implementeren per ticket, dan een
   review-pass, dan pas committen.
