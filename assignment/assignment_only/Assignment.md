# VIII. Praktická implementace: Modulární testování architektury Flexbox

Tato specifikace definuje strukturu testovací webové stránky rozdělené do pěti nezávislých sekcí. Každá sekce je navržena jako izolovaný modul, jehož cílem je verifikovat pochopení a správnou aplikaci specifické podmnožiny vlastností modulu Flexbox. Řešení nesmí využívat CSS Grid ani absolutní pozicování pro strukturální rozvržení.

## Modul 1: Horizontální navigační panel (Distribuce prostoru a zarovnání)

Tento modul testuje základní jednorozměrný tok a schopnost manipulovat se zbytkovým prostorem na hlavní ose.

**DOM struktura:** Kontejner `nav` obsahující tři logické bloky – `div.logo`, `ul.nav-links` (seznam odkazů) a `div.actions` (tlačítka pro přihlášení).

### Kritéria implementace

- Kontejner musí využívat horizontální tok (`flex-direction: row`).
- Veškeré prvky musí být vertikálně vycentrovány na křížové ose (`align-items: center`).
- Logické bloky musí být rozloženy tak, aby `logo` bylo ukotveno na levém okraji a `actions` na pravém okraji. Blok `nav-links` musí být umístěn mezi nimi.
- Požadavek na techniku: K odtlačení bloku `actions` na pravý okraj je vyžadováno použití vlastnosti `margin-left: auto`, nikoliv plošné rozdělení přes `justify-content`.

## Modul 2: Zobrazení hlavní upoutávky (Absolutní centrování)

Tento modul testuje schopnost Flexboxu řešit historicky nejkomplexnější problém CSS – dokonalé vycentrování prvku v obou osách současně.

**DOM struktura:** Kontejner `section.hero` s definovanou minimální výškou (`min-height: 400px`) obsahující jeden obalový `div.hero-content` (obsahující nadpis a podnadpis).

### Kritéria implementace

- Kontejner `section.hero` musí být definován jako flex kontejner.
- Prvek `div.hero-content` musí být vycentrován geometricky přesně uprostřed nadřazeného kontejneru, a to nezávisle na množství textu, které obsahuje.
- Vycentrování musí být dosaženo výhradně kombinací vlastností `justify-content` a `align-items` na úrovni rodičovského kontejneru.

## Modul 3: Responzivní produktová galerie (Zalamování a mikro-prostor)

Tento modul verifikuje chování vlastnosti `flex-wrap` a schopnost prvků dynamicky vyplňovat nebo uvolňovat prostor ve více řádcích.

**DOM struktura:** Kontejner `div.gallery-grid` obsahující 6 až 8 prvků `article.product-card`.

### Kritéria implementace

- Kontejner musí umožňovat zalamování prvků do nových řádků (`flex-wrap: wrap`).
- Distribuce mezer mezi kartami musí být řešena výhradně pomocí vlastnosti `gap` (např. `20px`).
- Každá karta musí mít definován ideální počáteční rozměr pomocí `flex-basis` (např. `250px`).
- Karty musí být schopné růst (`flex-grow: 1`), aby v případě zalomení posledního neúplného řádku plynule vyplnily zbývající volný prostor.

## Modul 4: Srovnávací cenové karty (Zarovnání potomků a vyrovnání výšky)

Tento modul kombinuje horizontální layout pro vnější rozvržení s vertikálním flexboxem pro vnitřní uspořádání komponent, čímž testuje izolaci flex kontextů.

**DOM struktura:** Kontejner `section.pricing` obsahující tři prvky `div.plan-card`. Každá karta obsahuje nadpis, seznam funkcí (různě dlouhý) a tlačítko `button.buy-btn`.

### Kritéria implementace

- Vnější kontext: Tři karty musí stát vedle sebe. Flexbox nativně zajistí, že všechny tři karty budou mít identickou výšku (rovnou výšce karty s nejdelším seznamem funkcí) díky výchozímu chování `align-items: stretch`.
- Vnitřní kontext: Samotná karta `div.plan-card` musí být definována jako vertikální flex kontejner (`flex-direction: column`).
- Tlačítko uvnitř karty musí být vždy ukotveno k jejímu spodnímu okraji, i když je seznam funkcí nad ním krátký. Toho je nutné dosáhnout aplikací `margin-top: auto` na samotné tlačítko, případně nastavením `flex-grow: 1` na oblast se seznamem funkcí.

## Modul 5: Multimediální komponenta typu "Komentář" (Ochrana proporcí)

Tento modul testuje porozumění vlastnosti `flex-shrink` a distribuci volného prostoru při kombinaci fixních a fluidních prvků.

**DOM struktura:** Kontejner `div.media-object` obsahující obrazový prvek `img.avatar` a textový obal `div.comment-body`.

### Kritéria implementace

- Avatar má definovanou pevnou šířku a výšku (např. `64px`).
- Při nedostatku horizontálního prostoru (např. na úzkém displeji) se avatar nesmí za žádných okolností zmenšit nebo deformovat. Pro zachování jeho proporcí je nutné aplikovat pravidlo `flex-shrink: 0`.
- Obal pro text (`div.comment-body`) musí naopak absorbovat veškerý zbývající prostor (`flex-grow: 1`) a umožnit zalamování textu uvnitř.
- Avatar a text musí být zarovnány k hornímu okraji (křížová osa) pomocí `align-items: flex-start`, aby při delším textu avatar nezůstal viset uprostřed vertikálního prostoru.

## Metodika hodnocení a validace

Správnost implementace je nezbytné ověřovat simulací stresových podmínek v uživatelském rozhraní:

- Test redukce viewportu: Zmenšování okna prohlížeče musí ve všech modulech vyvolat plynulou reorganizaci prostoru bez horizontálního přetečení obsahu (tzv. overflow).
- Test asymetrie obsahu: Vložení nestandardně velkého objemu textu do jednoho z prvků (např. do jedné cenové karty) musí automaticky ovlivnit rozměry a zarovnání sousedních prvků ve stejném flex kontextu.
- Inspekce DOMu: Ověření, že nebyly použity redundantní obalové prvky (tzv. div soup) pro dosažení zarovnání, které lze řešit nativními vlastnostmi Flexboxu.

---

Přejete si k těmto modulům vygenerovat validní CSS kód sloužící jako referenční autorské řešení?
