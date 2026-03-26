# Flexbox - Komplexní Průvodce

## I. Úvod a Definice

CSS Flexible Box Layout Module (zkráceně Flexbox) je jednorozměrný model pro rozmístění prvků na webové stránce, definovaný konsorciem W3C. Tento modul byl navržen pro optimalizaci distribuce volného prostoru mezi prvky v kontejneru a pro efektivní zarovnání těchto prvků, a to i v případech, kdy jejich rozměry nejsou předem známy nebo jsou dynamické (odtud označení "flexibilní").

Fundamentální problém, který Flexbox řeší, spočívá v omezeních historických metod pro tvorbu layoutu (tzv. Normal Flow). Před příchodem Flexboxu se rozvržení stránky realizovalo primárně pomocí vlastností `float`, `position` nebo dokonce HTML tabulek. Tyto metody byly určeny pro formátování textových dokumentů, nikoliv pro komplexní aplikační rozhraní. Flexbox eliminuje nutnost používat nestandardní "hacky" (např. clearfix) a nativně řeší chronické problémy CSS, jako je:

- Vertikální centrování obsahu.
- Automatické vyrovnání výšky sousedních sloupců.
- Rovnoměrné rozdělení zbývajícího prostoru nezávisle na pevných rozměrech v pixelech.

## II. Architektura a Principy

Základní architektura Flexboxu je postavena na principu izolovaného kontextu formátování (Flex Formatting Context). Jakmile je element deklarován jako flex kontejner, jeho přímí potomci se přestávají řídit standardním blokovým nebo řádkovým modelem (Block/Inline model) a stávají se flex položkami.

### Srovnání přístupů k layoutu:

- **Block Layout**: Prvky se skládají vertikálně pod sebe, optimalizováno pro textové dokumenty.
- **Float / Positioning**: Vytrhávání prvků z běžného toku dokumentu; obtížná kontrola nad výškou rodičovských kontejnerů.
- **Flexbox (1D Layout)**: Optimalizováno pro jednorozměrné uspořádání (buď v řádku, nebo ve sloupci). Rozděluje prostor na jedné dominantní ose.
- **CSS Grid (2D Layout)**: Optimalizováno pro dvourozměrné uspořádání (řádky i sloupce současně).

### Vnitřní mechanismy zpracování:

Algoritmus prohlížeče zpracovává flex layout ve dvou fázích:

1. **Výpočet základních rozměrů**: Prohlížeč analyzuje flex položky a zjistí jejich preferovanou velikost (základní rozměr před aplikací elasticity) na základě obsahu nebo explicitně definovaných rozměrů.

2. **Distribuce prostoru**: Prohlížeč vypočítá celkový dostupný prostor v kontejneru. Následně aplikuje koeficienty elasticity (smršťování nebo roztahování) a případný zbytek volného prostoru rozdělí do mezer podle pravidel pro zarovnání.

## III. Klíčové koncepty

Pochopení Flexboxu vyžaduje striktní rozlišení mezi osami a hierarchií prvků.

### 1. Hierarchie:

- **Flex kontejner (Flex Container)**: Rodičovský element, na kterém je aplikováno pravidlo `display: flex` nebo `display: inline-flex`.
- **Flex položka (Flex Item)**: Každý přímý potomek flex kontejneru.

### 2. Souřadnicový systém (Osy):

Na rozdíl od standardního souřadnicového systému X a Y používá Flexbox relativní osy, které se mění v závislosti na směru toku obsahu.

- **Hlavní osa (Main Axis)**: Primární osa, podél které jsou rozmisťovány flex položky. Její směr určuje vlastnost `flex-direction`.
- **Křížová osa (Cross Axis)**: Osa, která je vždy kolmá na hlavní osu.

### 3. Terminologie prostoru:

- **Main Start / Main End**: Začátek a konec kontejneru na hlavní ose.
- **Cross Start / Cross End**: Začátek a konec kontejneru na křížové ose.

### 4. Vlastnosti Flex kontejneru:

- **flex-direction**: Určuje směr hlavní osy (`row`, `row-reverse`, `column`, `column-reverse`).
- **flex-wrap**: Definuje, zda položky přetečou kontejner, nebo se zalomí do nového řádku/sloupce (`nowrap`, `wrap`, `wrap-reverse`).
- **justify-content**: Řídí zarovnání a distribuci prostoru na hlavní ose (`flex-start`, `center`, `space-between`, `space-around`, `space-evenly`).
- **align-items**: Řídí zarovnání položek na křížové ose pro aktuální řádek/sloupec.
- **align-content**: Řídí distribuci prostoru mezi více řádky/sloupci na křížové ose (má efekt pouze v kombinaci s `flex-wrap: wrap`).

### 5. Vlastnosti Flex položky (Elasticita):

Pro pochopení elasticity je vhodná analogie s pružinami.

- **flex-basis**: Výchozí velikost položky před distribucí zbývajícího prostoru (ideální délka pružiny v klidovém stavu).
- **flex-grow**: Bezrozměrné číslo určující schopnost položky růst a zabrat volný prostor na hlavní ose v poměru k ostatním položkám (síla, kterou pružina expanduje).
- **flex-shrink**: Bezrozměrné číslo určující schopnost položky zmenšit se, pokud je celková velikost položek větší než kontejner (ochota pružiny nechat se stlačit).

## IV. Praktická příprava a Konfigurace

Před implementací Flexbox layoutu je nezbytné zajistit stabilní prostředí a správnou strukturu DOMu.

### 1. DOM Struktura:

Flexbox operuje striktně na vztahu rodič-přímý potomek. Prvky, které jsou vnořeny hlouběji (potomci potomků), nejsou vlastnostmi hlavního flex kontejneru ovlivněny.

### 2. Normalizace Box Modelu:

Základním předpokladem pro předvídatelné chování rozměrů je sjednocení box modelu napříč celým dokumentem.

```css
*, *::before, *::after {
  box-sizing: border-box;
}
```

### 3. Odstranění marginů:

Při definici celostránkového layoutu je standardní praxí eliminovat výchozí odsazení prohlížeče, aby kontejner mohl bez problému zabrat celou plochu viewportu:

```css
body {
  margin: 0;
  min-height: 100vh;
}
```

### 4. Bezpečnostní pravidla:

Ve flex kontextu nefungují vlastnosti jako `float`, `clear` a `vertical-align`. Pokud jsou na flex položku aplikovány, prohlížeč je ignoruje.

## V. Workflow / Proces

Standardní postup pro vytvoření rozvržení pomocí Flexboxu sestává z následujících sekvenčních kroků:

### Aktivace kontextu:

Přidělení role kontejneru.

```css
.container { display: flex; }
```

### Definice směru (Hlavní osa):

Určení, zda budou položky vedle sebe, nebo pod sebou.

```css
.container { flex-direction: column; }
```

### Nastavení zalamování:

Rozhodnutí, jak systém zareaguje na nedostatek místa.

```css
.container { flex-wrap: wrap; }
```

### Distribuce prostoru na hlavní ose:

Zarovnání položek do volného prostoru.

```css
.container { justify-content: space-between; }
```

### Zarovnání na křížové ose:

Kontrola vertikální (nebo horizontální) pozice vůči výšce řádku.

```css
.container { align-items: center; }
```

### Individuální úpravy položek (volitelné):

Úprava chování specifických položek, definice elasticity nebo přepsání zarovnání pomocí `align-self`.

```css
.item-highlight { flex: 1 1 auto; align-self: flex-start; }
```

## VI. Pokročilé techniky a Řešení problémů

### 1. Shorthand vlastnost flex:

V praxi se pro nastavení elasticity položek téměř výlučně používá zkratka `flex`, která kombinuje `flex-grow`, `flex-shrink` a `flex-basis`.
Doporučený zápis: `flex: 1 1 0%;` (položka roste, zmenšuje se a její výchozí základ je nulový – rozdělí prostor rovnoměrně).

### 2. Efekt margin: auto ve Flexboxu:

Aplikace vlastnosti `margin: auto` na flex položku způsobí pohlcení veškerého dostupného volného prostoru v daném směru. Tato technika se běžně využívá pro odsunutí jedné položky (např. tlačítka "Logout" v navigaci) na opačnou stranu kontejneru.

### 3. Kolaps marginů (Margin Collapsing):

Ve standardním blokovém toku se sousední vertikální okraje (margin) překrývají a splývají v jeden větší. Ve Flexboxu k žádnému kolapsu okrajů nedochází. Marginy všech flex položek se vždy sčítají, což zjednodušuje matematiku mezer.

### 4. Kritická chyba: Přetečení obsahu (min-width: 0):

**Problém**: Flex položka může přetéct přes hranice kontejneru, pokud obsahuje nepřerušitelný text (např. dlouhou URL) nebo nezmenšený obrázek. Děje se tak proto, že výchozí hodnota velikosti je `auto`, nikoliv `0`.

**Řešení**: Vynucení minimální šířky pro flex položky obsahující textové/obrazové uzly, aby algoritmus umožnil jejich zmenšení pod velikost samotného obsahu.

```css
.flex-item {
  min-width: 0; /* Pro flex-direction: row */
  min-height: 0; /* Pro flex-direction: column */
}
```

## VII. Ekosystém a Nástroje

Teoretický model Flexboxu je implementován nativně přímo v renderovacích jádrech prohlížečů, nicméně vývojový proces je často urychlován externími nástroji a abstrakcemi.

### 1. CSS Preprocesory a Autoprefixer:

V minulosti vyžadovaly různé prohlížeče tzv. vendor-prefixy (např. `-webkit-box`, `-ms-flexbox`). Součas ný ekosystém tento problém řeší automatizovaně prostřednictvím nástrojů jako PostCSS a jeho pluginu Autoprefixer. Vývojář píše čistý standardní kód (`display: flex`) a buildovací proces doplňuje prefixy pouze tam, kde to vyžaduje cílová matice prohlížečů (definovaná například v souboru `.browserslistrc`).

### 2. Vývojářské nástroje prohlížečů (DevTools):

Moderní prohlížeče (Chrome, Firefox, Edge) obsahují dedikované inspektory pro Flexbox. V DOM struktuře je u flex kontejneru speciální ikona, která po kliknutí vizualizuje hraniční čáry, volný prostor a aktuálně aplikované osy přímo ve vrstvě nad renderovanou stránkou.

### 3. Abstrakce ve frameworkcích:

Ucelené sady CSS utilit kompletně zapouzdřují složitost Flexboxu do jednoduchých tříd:

- **Tailwind CSS**: Používá třídy mapující jedna k jedné na CSS vlastnosti (např. `flex`, `flex-row`, `justify-center`, `items-center`).
- **Bootstrap**: Poskytuje robustní gridový systém, který interně využívá matematiku Flexboxu pro zarovnání a automatickou distribuci šířky sloupců (třídy jako `d-flex`, `align-items-start`).

## VIII. Praktické procvičení a zdroje

Pro upevnění teoretických znalostí a nácvik reálné implementace je vyžadována práce s následujícími výukovými materiály a nástroji.

### 1. Komplexní aplikační úloha (GitHub repozitář)

Referenční implementace a prostředí pro vývoj komplexního layoutu se nachází v dedikovaném repozitáři na platformě GitHub: https://github.com/L0SPAVL0S/OM-WEB-Flexbox.

- **Výchozí kódová základna**: V adresáři `assignment/assignment_only` jsou připraveny výchozí soubory bez implementované logiky rozvržení.
- **Specifikace úlohy**: Detailní technické požadavky na vypracování layoutu jsou definovány v dokumentu `assignment.md`. Obsahuje přesné pokyny pro aplikaci modulárních pravidel.
- **Vzorové řešení**: Repozitář poskytuje kompletně vypracované referenční řešení pro křížovou kontrolu a studium správné struktury zápisu.

### 2. Gamifikované testování syntaxe (Flexbox Froggy)

Pro izolované procvičování zápisu vlastností a distribuce prostoru je určen interaktivní výukový nástroj Flexbox Froggy, dostupný na adrese: https://flexboxfroggy.com/#cs.

Jedná se o platformu simulující 24 vizuálních úloh s rostoucí obtížností. Metodika spočívá v deklaraci validních CSS pravidel (např. `justify-content`, `align-items`, `flex-wrap`) pro dosažení topologické shody mezi zdrojovým elementem a požadovanou pozicí v kontejneru. Nástroj slouží k okamžité vizuální zpětné vazbě na chování jednotlivých algoritmů Flexboxu.
