---
title: Architektura FloWorks
description: Přehled komponent a vnitřního fungování pro koncového uživatele
---

# Architektura FloWorks – Pohled pro uživatele

FloWorks je desktopová aplikace, která vám umožňuje stavět řetězce zpracování signálů prostřednictvím vývojových diagramů. Propojte bloky (uzly) na interaktivním plátně a sledujte výsledky v reálném čase. Aby to bylo možné, je aplikace organizována do několika modulů, které spolupracují. Níže je vysvětleno bez technických detailů, co dělá každá část a jak spolu souvisejí.

---

## Obecná struktura

Aplikace se skládá z následujících funkčních oblastí:

| Oblast | Co dělá? |
|------|------------|
| **Spuštění a hlavní okno** | Spustí program, zobrazí okno, nabídky a koordinuje všechny uživatelské akce. |
| **Výpočetní jádro** | Vypočítá pořadí, v jakém se uzly mají spustit, detekuje závislosti a smyčky a přenáší data z uzlu na uzel. |
| **Scéna a diagram** | Spravuje plátno, kam umisťujete uzly, propojení mezi nimi, poznámky a akce zpět/vpřed. |
| **Uzly a zpracování** | Obsahuje všechny typy bloků, které můžete použít: zdroje signálů, matematické operace, vlastní skripty, export grafů atd. |
| **Vizuální konektory** | Kreslí čáry spojující uzly (hladké křivky nebo ortogonální), animuje je pro zobrazení toku dat a zabraňuje jejich překrývání. |
| **Uživatelské rozhraní** | Zahrnuje zobrazení diagramu (zoom, posun), panel nástrojů, tabulku parametrů, analytické panely (statistiky, kurzory) a konfigurační dialogy. |
| **Podpora reálného hardwaru** | Umožňuje komunikaci s laboratorními přístroji (osciloskopy, generátory, LCR multimetry) pro zachycení nebo generování reálných signálů. |
| **Export grafů** | Generuje obrázky ve vysoké kvalitě (PNG, PDF, SVG) s plnou vizuální přizpůsobitelností. |
| **Témata a vzhled** | Mění vzhled celé aplikace (tmavý, světlý, vysoký kontrast) a umožňuje nastavit velikost písma. |
| **Jazyky** | Překládá celé rozhraní do více jazyků a umožňuje změnu jazyka okamžitě. |
| **Správa projektů** | Ukládá a otevírá soubory `.sflow` s celým diagramem, včetně konfigurací, skriptů a výsledků. |
| **Testy a diagnostika** | Vnitřní nástroje pro ověření správné funkčnosti (neviditelné pro koncového uživatele). |

---

## Jak to funguje uvnitř

### Spuštění a hlavní okno
Po otevření FloWorks se nakonfiguruje grafické prostředí, detekuje se hustota pixelů vaší obrazovky (aby vše vypadalo ostře na 4K i běžných monitorech) a zobrazí se hlavní okno. Toto okno centralizuje všechny prvky: kreslicí plochu, nabídky, panel nástrojů a boční panely.

### Tokové jádro
Když stisknete "Spustit" (nebo F5), vnitřní jádro projde všechny uzly ve správném pořadí s respektováním propojení. Ví, které uzly závisejí na jiných, a zabraňuje nekonečným smyčkám. Podporuje, aby uzel přijímal více pojmenovaných vstupů a produkoval více výstupů. Data mezi uzly cestují bez ztráty původní struktury.

### Scéna diagramu
Plátno, na kterém stavíte své diagramy, je inteligentní scéna:

- Umožňuje přidávat uzly, přesouvat je, propojovat je a vybírat je.
- Podporuje neomezené zpět/vpřed pro jakoukoli akci.
- Zahrnuje měnitelné poznámky, které můžete volně umisťovat a které se ukládají s projektem.
- Má automatický uspořádávač, který přeskupí uzly uspořádaně (pomocí Ctrl+Shift+L).
- Při ukládání se celý diagram zabalí do souboru `.sflow`, který obsahuje popisy uzlů, propojení, poznámky a přidružená číselná data.

### Konektory
Čáry spojující uzly se kreslí jako hladké křivky nebo ortogonální trasy. Jemná animace teček nebo čerchalvání ukazuje směr toku. Správce pruhů zabraňuje tomu, aby se více spojení mezi stejnými uzly hromadilo; oddělí je automaticky, aby vše bylo čitelné.

### Typy uzlů
Uzly jsou základními stavebními kameny. Dělí se do tří kategorií:

- **Zdroje** – Generují signály. Mohou simulovat vlny (sinus, obdélník atd.) nebo číst reálná data z připojeného osciloskopu či multimetru. Podporují více kanálů současně (např. impedance a fáze z LCR).
- **Zpracování** – Transformují data. Zahrnují aritmetické operace (sčítání, odčítání, násobení, dělení), podmíněná rozhodnutí (větev Ano/Ne) a výkonný skriptovací uzel, který vám umožňuje psát vlastní Python kód s vizuálními pomůckami.
- **Výstupy** – Zobrazují nebo exportují výsledky. Nejčastější je grafický vizualizér (virtuální osciloskop), ale existuje také exportér grafů profesionální kvality.

Každý uzel má vstupní porty (vlevo/nahoře) a výstupní porty (vpravo/dole). Propojením výstupního portu se vstupním signál mezi nimi proudí.

#### Pokročilý skriptovací uzel
Skriptovací uzel si zaslouží zvláštní zmínku. Je určen pro pokročilé uživatele, kteří chtějí přidat vlastní zpracování bez opuštění FloWorks. Nabízí:

- Editor se zvýrazňováním syntaxe, automatickým doplňováním a číslováním řádků.
- Možnost definovat parametry upravitelné z panelu uzlu bez nutnosti sahat do kódu (např. číselná hodnota použitá ve skriptu).
- Dynamické vstupní a výstupní porty: přidáním speciálních komentářů ve skriptu můžete vytvářet nové konektory.
- Trvalou paměť: speciální proměnná (`persist`), která si zachovává svou hodnotu mezi spuštěními, užitečná pro akumulátory nebo stavové automaty.
- Připravené šablony skriptů a možnost uložit si vlastní.
- Integrovaný systém nápovědy a konzole zobrazující chyby při běhu.

### Uživatelské rozhraní
Kromě plátna rozhraní zahrnuje:

- **Panel nástrojů** se všemi uzly uspořádanými podle kategorií, nabídkami jazyka, tématu a velikosti písma a přístupem k prohlížeči logů.
- **Tabulku parametrů** zobrazující informace o vybraných uzlech a zvýrazňující možné neslučitelnosti (jako pokus o operaci se signály různé délky).
- **Připojitelné analytické panely**: statistiky (maximum, minimum, efektivní hodnota), kurzory A/B pro měření rozdílů a zaměřovač se značkou vrcholu.
- **Uvítací dialog**, který se přizpůsobí rozlišení vaší obrazovky a nabízí úvodní možnosti.

### Připojení k reálným přístrojům
Pokud máte kompatibilní hardware (osciloskopy Siglent SDS, LCR multimetry, generátory SDG), FloWorks s nimi může komunikovat prostřednictvím standardního protokolu VISA/SCPI. Konfigurace se provádí z konkrétních panelů v aplikaci. Když zachytíte vícekanálový signál (např. velikost a fázi z LCR), zdrojový uzel zabalí všechny kanály a vy si můžete vybrat, který zobrazit pomocí jednoduché kontextové nabídky.

### Profesionální export grafů
Exportér grafů vám umožňuje generovat obrázky připravené pro zprávy nebo publikace. Dvojitým klikem na něj se otevře dialog s mnoha možnostmi: můžete přizpůsobit barvy, typy čar, popisky, měřítka, volit mezi formáty PNG, PDF nebo SVG a uložit své preference jako znovupoužitelné profily.

### Vizuální přizpůsobení
FloWorks obsahuje několik témat (tmavé, světlé, vysoký kontrast), které okamžitě mění vzhled celého rozhraní bez restartu. Navíc můžete nastavit globální velikost písma z nabídky (Informace → Velikost písma) a všechny prvky se přizpůsobí, včetně textů uvnitř uzlů, poznámek a grafů.

### Jazykový systém
Aplikace automaticky detekuje jazyk vašeho systému při prvním spuštění a uloží vaši preferenci. Jazyk můžete kdykoli změnit z nabídky; všechny texty, nabídky a nápovědy se aktualizují za běhu.

### Projekty a soubory `.sflow`
Veškerá vaše práce se ukládá do jediného souboru s příponou `.sflow`. Tento soubor obsahuje celý diagram: uzly, propojení, poznámky, konfigurace, skripty a vygenerovaná číselná data. Můžete jej sdílet s ostatními uživateli; při otevření na jiném počítači se poznámky a uzly automaticky přeškálují podle hustoty pixelů té obrazovky.

---

## Typické pracovní postupy

1. **Vytvoření jednoduchého diagramu**  
   Vyberte zdrojový uzel (např. Generátor) a vizualizační uzel z panelu nástrojů.  
   Propojte výstup generátoru se vstupem vizualizéru (Ctrl+klik na výstupní port, pak klik na vstupní).  
   Stiskněte F5 pro spuštění. Signál uvidíte v grafu.

2. **Použití vlastního skriptu**  
   Přidejte uzel Skript.  
   Napište svůj Python kód v editoru; můžete definovat upravitelné parametry a extra porty.  
   Propojte jeho vstupy a výstupy jako jakýkoli jiný uzel.  
   Spusťte tok; skript se zpracuje s vašimi daty.

3. **Zachycení dat z reálného osciloskopu**  
   Připojte přístroj a nakonfigurujte komunikaci z panelu uzlu Osciloskop.  
   Uzel získá signál a předá jej přes své výstupní porty (jeden na kanál).  
   Propojte tyto porty s dalšími zpracovatelskými uzly nebo s vizualizérem.

4. **Export grafu pro zprávu**  
   Připojte požadovaný signál k uzlu Exportér grafů.  
   Vyberte v uzlu (pravý klik) pro konfiguraci vizuálního vzhledu grafu.  
   Lze také načíst/uložit profily pro zrychlení získávání grafů připravených pro zprávy a získat obrazový soubor ve zvolené příponě.

---

## K čemu to slouží

Tato architektura je navržena tak, abyste se mohli soustředit na analýzu signálů bez obav z vnitřní organizace programu. Každá komponenta má jasnou funkci a společně nabízejí plynulý zážitek, od simulace po reálnou instrumentaci, přes vizuální přizpůsobení až po export výsledků.

Pokud někdy budete potřebovat rozšířit schopnosti FloWorks (např. přidáním nových typů uzlů nebo připojením jiného přístroje), vězte, že existuje modulární struktura, která to umožňuje, i když to je doména pro vývojáře. Jako koncový uživatel si užijte flexibilitu, kterou vám toto řešení poskytuje.
