# Filozofie

## Přehled

FloWorks je desktopová aplikace, kter vám umožňuje vytvářet řetězce zpracování signálů pomocí vizuálních vývojových diagramů.
Přetahujte, propojujte a konfigurujte uzly; výsledek se vypočítá a zobrazí v reálném čase.
Pracujte se simulovanými signály nebo připojte reálné přístroje (osciloskopy, generátory, multimetry LCR) bez nutnosti psát kód, ačkoli máte k dispozici výkonné skriptovací prostředí, pokud chcete rozšířit funkčnost.

---

## Hlavní vlastnosti

- **Interaktivní diagramy** – Sestavte svůj pracovní postup propojením uzlů čarami, které reprezentují tok dat.
- **Zpracování v reálném čase** – Každá změna se okamžitě projeví v grafech a vizualizacích.
- **Simulace a reálný hardware** – Generujte testovací signály nebo zachytávejte data přímo z laboratorních přístrojů.
- **Pokročilý skriptovací uzel** – Včleňte svůj vlastní kód Python s nápovědou pro autodoplňování, editovatelnými dynamickými parametry a pamětí mezi spuštěními.
- **Profesionální vizualizace** – Signály, spektra, spektrogramy a grafy vysoké kvality připravené k exportu.
- **Vícejazyčnost** – Rozhraní detekuje jazyk systému a umožňuje přepínat mezi španělštinou, angličtinou a dalšími jazyky kdykoli.
- **Vizuální témata** – Tmavý, světlý a vysokokontrastní režim pro přizpůsobení vašim preferencím nebo potřebám přístupnosti.
- **Komplexní správa projektů** – Uložte svou práci do souborů `.sflow` a obnovte ji přesně tak, jak jste ji opustili, s neomezeným zpětným a znovu provedením kroků.

---

## Jak pracovat s FloWorks

### Uzly
Uzel je část zpracování. Jsou uspořádány do tří kategorií:

- **Zdroje** – Vkládají signály na začátek toku. Například osciloskop (reálný nebo simulovaný), generátor funkcí nebo matematická operace.
- **Zpracování** – Transformují data. Součty, rozdíly, podmínky, filtry… včetně speciálního uzlu pro psaní vlastních skriptů v Pythonu.
- **Výstupy** – Zobrazují nebo exportují výsledky. Grafický vizualizér a profesionální exportér grafů jsou nejpoužívanější.

### Připojení
Spojení mezi uzly se kreslí jako hladké křivky nebo ortogonální čáry. Animace toku vám v každém okamžiku ukazuje směr dat. Systém automaticky uspořádává kabely tak, aby se nepřekrývaly.

### Vizualizace
Kdykoli uzel vyprodukuje signál, lze jej zobrazit v integrovaném grafickém panelu. Můžete prozkoumávat různá zobrazení (průběh, spektrum, spektrogram) a upravovat měřítko pomocí myši.

---

## Vybrané uzly
Jsou to minimální nezbytné uzly, které jsou nutné, aby filozofie programu dávala smysl.

### Uzel generátoru signálů
Zdroj signálů, který může generovat uživatelem definované simulace průběhů. Umožňuje z kontextové nabídky vybrat nebo zadat požadovaný průběh.

### Skriptovací uzel
Kompletní programovací prostředí uvnitř diagramu:

- **Editor se zvýrazňováním syntaxe**, autodoplňováním a chybovou konzolí.
- **Dynamické parametry** – Definujte editovatelné proměnné z panelu uzlu bez úpravy kódu.
- **Konfigurovatelné porty** – Přidávejte další vstupy a výstupy přímo z editoru.
- **Trvalý stav** – Ukládejte hodnoty mezi spuštěními; vše se ukládá spolu s projektem.

### Exportér grafů
Výstupní uzel, který generuje obrázky vysoké kvality pro zprávy nebo publikace. Umožňuje konfigurovat velikost, rozlišení, formát a další.

---

## Přizpůsobení

- **Jazyk** – Aplikace automaticky detekuje jazyk systému a uloží vaši preferenci. Můžete jej změnit z nabídky bez restartu.
- **Vzhled** – Vyberte si mezi tmavým, světlým nebo vysokokontrastním motivem podle okolního osvětlení nebo svých vizuálních potřeb.

---

## Projekty a soubory

Uložte svůj kompletní diagram do souboru `.sflow`.
Po otevření obnovíte všechny uzly, připojení, skripty, parametry a konfigurace vizualizace.
Akce zpět a znovu vám umožní experimentovat bez obav ze ztráty předchozí práce.

---

FloWorks je navržen tak, abyste se mohli soustředit na analýzu signálů, a ne na technické detaily implementace. Přetahujte, propojujte a objevujte.
