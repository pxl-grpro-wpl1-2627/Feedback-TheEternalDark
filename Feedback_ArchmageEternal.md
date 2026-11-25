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

- **Alle 13 tickets werken.** De drie `Has...`-properties staan op `Player`, staan
  in de constructor op `false`, en `TryFindSpecialItem()` zet de juiste property
  en geeft `null` terug als er niets gevonden is.
- **Ik begrijp de structuur van het project.** Ik wist welke klasse welke
  verantwoordelijkheid heeft, en ik heb de nieuwe code op de juiste plaats gezet:
  de hond raakt `Biome`, de traan en het oog raken `Player`, de voorwerpen raken
  `World`. Daardoor heb ik geen gegeven bestand moeten aanpassen.
- **Ik heb strikt per ticket gewerkt.** Eerst DOG, dan TEAR, dan EYE, dan FIND.
  Toen iets niet klopte, wist ik meteen in welke commit het zat.
- **Ik heb het spel volledig getest**, drie keer helemaal opnieuw, in plaats van
  alleen te vertrouwen op "het compileert".
- **`EffectiveSunCost` is op alle drie de plaatsen gebruikt** in `BuildSun()`
  (controle, aftrekken, foutboodschap) en de kostprijs kan nooit negatief worden.

---

## 3. Wat minder goed ging

| # | Probleem | Waar | Wat de lector doet |
| --- | --- | --- | --- |
| 1 | **Dubbele code**: de logica om distinct letters te kiezen staat twee keer (constructor én `GenerateBiomeSeed`). | `World.cs` | Eén private `PickDistinctLetters(string pool, int count)`, voor beide gevallen gebruikt. |
| 2 | **Verouderde commentaar**: de bestandskop zegt nog "Standard solution for iteration 1". | alle drie | Commentaar beschrijft de huidige code, inclusief waarom. |
| 3 | **Taalfout in een spelbericht**: `Name + " You are already at camp."` mist het werkwoord "is". | `Player.ReturnToCamp()` | `Name + " is already at camp."` |
| 4 | **Spelfouten** in spelteksten: "alredy shinning", "You need go back", "thats finally pretty warm right here". | `Player.BuildSun()` | Neutrale, correcte zinnen. |
| 5 | **Dode code**: `Location = 0;` in de branch waarin de speler al op kamp staat. | `Player.ReturnToCamp()` | Die regel staat er niet. |
| 6 | **Random-stijl**: `rnd.Next(1, ItemFindChance + 1) == 1` is correct, maar minder leesbaar dan de 0-gebaseerde variant. | `World.TryFindSpecialItem()` | `rnd.Next(0, ItemFindChance) == 0` |

Wat mij zelf het meest stoort: **ik gebruik alleen de eenvoudigste methodes die
ik ken.** Ze werken, maar ze zijn niet altijd de beste keuze voor de situatie.
Ik heb geen "gereedschapskist" waaruit ik kies; ik pak altijd hetzelfde stuk
gereedschap omdat ik de rest niet vlot kan schrijven.

De lectoroplossing is op die punten beter. Dat is ook logisch: hij schrijft deze
code al jaren en heeft dezelfde soort opgave al tientallen keren opgelost. Het
verschil zit in routine en toepassing, niet in iets wat ik niet zou kunnen leren.

---

## 4. Waarom ging het minder goed?

**a) Ik heb de les vlak voor deze PE gemist.** Door familieomstandigheden was ik
niet aanwezig in de les waarin deze leerstof werd uitgelegd. Ik heb dat thuis
opgevangen met online lessen en met AI, en zo heb ik de basis wel gehaald: alle
tickets werken. Maar video's kijken is passief leren. Ik herken de code wanneer
ik ze zie, maar ik kan ze niet zelf reproduceren in een situatie die ik niet
letterlijk gezien heb.

**b) Ik kon het laatste ticket niet "zien".** Bij FIND heb ik de opgave zeker
tien tot twintig keer herlezen en ik kon er geen volledig beeld van maken in mijn
hoofd. Ik wist wat er stond, maar niet hoe ik het moest opschrijven. Pas nadat ik
het in kleine stukjes knipte, kwam er beweging in. Dat kostte bijna al mijn tijd:
van de ongeveer 1 uur en 40 minuten is het grootste deel naar dat ene ticket
gegaan.

**c) Ik kopieer methodes uit mijn eigen oude code.** Als een nieuwe opgave
"ongeveer hetzelfde" voelt, neem ik een methode over uit een vorige oefening.
Maar de situatie is nooit precies dezelfde, dus dan sleep ik een oplossing mee
die niet past — en dat is precies waarom de logica in `World.cs` twee keer staat.

**d) Ik heb niemand om feedback gevraagd terwijl ik werkte.** Omdat ik de les had
gemist, had ik net tijdens het maken moeten vragen: "is dit de juiste manier?"
Dat heb ik niet gedaan, en dan herhaal je je eigen fouten.

**e) Taal.** Ik leer programmeren in het Nederlands, en dat is niet mijn
moedertaal. Programmeren vind ik zelfs in mijn eigen taal moeilijk; in het
Nederlands komt daar nog een laag bij. Vooral bij lange, formele opgavebeschrijvingen
kost het lezen mij meer tijd dan het schrijven van de code.

**f) Geen automatische tests.** Ik test manueel, dus fouten zoals de taalfout en
de dode regel glippen er door.

---

## 5. Wat ik meeneem naar de volgende keer

1. **Geen lessen meer missen.** Als het toch gebeurt: meteen de lector mailen en
   vragen wat ik moet inhalen, niet pas achteraf.
2. **Stoppen met methodes kopiëren uit oude code.** Eerst kijken wat de nieuwe
   situatie vraagt, dan pas schrijven. Voelt de opgave "hetzelfde", dan is dat
   juist het moment om extra op te letten.
3. **De opgave in stukjes knippen voor ik begin.** Bij FIND had ik eerst de
   deelvragen op papier moeten zetten; dan was het denkwerk kleiner geweest.
4. **Vragen stellen tijdens het werken**, niet na de deadline.
5. **Een review-pass voor de laatste commit:** commentaar bijwerken, dode regels
   verwijderen, spelteksten hardop lezen.

---

## 6. Wat ik al goed beheers

- **Structuur en verantwoordelijkheden**: welke klasse doet wat, en waar hoort
  nieuwe code thuis.
- **Properties en encapsulatie** — geen publieke velden, `get; set;` waar nodig.
- **Constructors** en het correct initialiseren van alle velden.
- **`const` vs. gewone velden**, en `private` vs. `public` constanten.
- **Methodes met parameters en returnwaarden**, en eenvoudige private
  hulpmethodes zoals `EffectiveSunCost`.
- **`Math.Max` / `Math.Abs`**, integer-deling en `Random` met kansberekening.
- **Stringmanipulatie**: indexering, `IndexOf`, `Substring`, `Remove`.
- **If/else met vroege returns.**

Ik los al een tijd problemen op via LeetCode, en dat merk ik wel: als ik de
logica eenmaal zie, kan ik ze opschrijven. Mijn beste momenten zijn de keren dat
ik de volledige code in één keer in mijn hoofd zie en hem gewoon kan uitschrijven.

---

## 7. Wat ik nog niet onder de knie heb

- **Methodes kiezen en refactoren.** Ik ken de theorie van hulpmethodes, maar ik
  pas ze niet spontaan toe. Dit is mijn grootste technische zwakte in deze PE.
- **Een complexe opgave omzetten naar code.** Bij een korte, duidelijke opgave lukt
  het; bij een lange beschrijving verlies ik het overzicht.
- **Nette, consistente output** — grammatica en toon in spelberichten.
- **Kleinschalig testen** met asserts in plaats van enkel manueel spelen.
- **Taal**: opgavebeschrijvingen in het Nederlands sneller en preciezer lezen.

Ik heb niet echt één C#-onderwerp dat ik "van nul opnieuw moet studeren". Het
probleem zit minder in de theorie dan in de toepassing: ik weet wat een methode
is, maar ik zie niet wanneer ik er zelf één moet maken.

---

## 8. Concreet plan

1. `World.cs` herschrijven met één `PickDistinctLetters(pool, count)` en opnieuw
   builden — zodat ik het verschil zelf heb toegepast.
2. De comments in alle drie de bestanden bijwerken naar iteratie 2 en de
   spelteksten herlezen.
3. **De gemiste leerstof inhalen bij de lector**: vragen wat ik heb gemist en of
   hij mijn herschreven `World.cs` wil nakijken.
4. **Actief oefenen in plaats van video's kijken**: twee oefeningen waarbij ik
   een lang blok zelf moet opsplitsen in methodes, en daarna vergelijken met een
   voorbeeldoplossing.
5. Bij de volgende PE: eerst ticketlijst en deelvragen op papier, dan per ticket
   implementeren, dan een review-pass, dan committen — en bij twijfel meteen
   vragen stellen.
