[Co dodělat ]: #
[pojmy ]: #

# Výběr řídící jednotky pro různé aplikace


$${\color{#FFA500}E9 \space \color{#4682B4}A1 }$$

## Cíle

- **Orientovat se** v základních typech a architekturách řídicích jednotek (MCU, MPU, embedded systémy, PLC, iPC, programovatelná relé).
- **Rozlišovat klíčové technické parametry** (výpočetní výkon vs. spotřeba, typy a velikosti pamětí RAM/Flash/EEPROM, determinismus a reakční doba v reálném čase).
- **Zhodnotit provozní odolnost a robustnost** hardwaru (krytí IP, teplotní rozsah, vibrace, rušení EMC, srovnání spotřební vs. průmyslové techniky).
- **Navrhnout a technicko-ekonomicky obhájit** optimální řídicí jednotku pro konkrétní praktickou aplikaci podle I/O bilance, rozhraní a prostředí.

## Ověření cílů

Výběr řídící jednotky pro různé aplikace

1. Příklady řídících jednotek 
2. Jejich základní vlastnosti z hlediska výpočetního výkonu a velikosti paměťového prostoru
3. A z hlediska odolnosti
4. Příklady použití v praxi (kde se používají MCU, a kde ř. j. s MPU)

%%
1. Správné vysvětlení pojmů, architektur a zkratek z oblasti řídicích systémů.
2. Schopnost posoudit vliv prostředí na výběr hardwaru a dešifrovat IP kód.
3. Vypracování rozhodovací matice pro volbu vhodné platformy (MCU vs. PLC vs. iPC).
4. Návrh konkrétní konfigurace řídicí jednotky na základě zadané I/O bilance a provozních podmínek.
5. Kritická technická oponentura (audit) nevhodně navrženého řešení.
%%


---

## Úlohy


### 1. Základní pojmy a architektury řídicích jednotek

*Časová dotace: 10–15 minut | Úvodní úloha*

Doplňte do níže uvedené tabulky význam zkratek, základní princip a typický příklad reálného nasazení nebo zástupce:

| Zkratka / Pojem          | Co zkratka znamená (česky/anglicky) | Základní charakteristika (architektura, kde běží program)                                 | Typický zástupce                  | Příklad nasazení                           |
| :----------------------- | :---------------------------------- | :---------------------------------------------------------------------------------------- | :-------------------------------- | ------------------------------------------ |
| **MCU**                  |  Microcontroller Unit (Mikrořadič)   | Integrovaný čip (CPU + RAM + Flash na jednom křemíku), deterministický běh bez OS / RTOS  | např. ESP32, PIC16LF1xxx, RP2040  |Senzory, domácí spotřebiče, hračky |
| **MPU**                  |  Microprocessor Unit (Mikroprocesor)| Samostatný procesor vyžadující externí RAM a úložiště, často běží plnohodnotný OS (Linux) |  ARM Cortex-A (např. Raspberry Pi), Intel Core |Tablety, pokročilé brány (IoT gateways), počítače |
| **Embedded**             |  Vestavěný systém |Účelově zaměřený počítačový systém kombinující hardware (MCU/MPU) a software pro specifickou úlohu | Embedded PLC, embedded PC         | Bílá technika, bankomaty, plynové kotle... |
| **PLC**                |  Programmable Logic Controller (Programovatelný logický automat)| Průmyslový automat pro cyklické řízení procesů, vysoká odolnost, modulární/kompaktní      | SIMATIC S7-1200/1500  |Průmyslové linky, výrobní stroje, robotika     |
| **iPC**                  |Industrial PC (Průmyslový počítač) |Počítač v průmyslovém provedení, vysoká výkonnost, x86/ARM architektura, běží průmyslový OS |Siemens SIMATIC IPC, Beckhoff |Vizualizace (HMI/SCADA), náročný sběr dat|
| **Programovatelné relé** |Smart relay / Programovatelné relé | Zjednodušené malé PLC pro méně náročné úlohy (nahrazuje časovače a relé)                  | např. Siemens LOGO!, Eaton easyE4 | Ovládání osvětlení, menší vzduchotechnika, brány    |

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **SoC (System on Chip):** Čip integrující CPU, GPU, paměť i bezdrátové moduly (např. Wi-Fi/BT) na jediném substrátu (např. v telefonech, ESP32).
> - **DSP (Digital Signal Processor):** Specializovaný procesor s architekturou optimalizovanou pro bleskové matematické operace (filtrace zvuku, FFT, řízení motorů).
> - **FPGA (Field-Programmable Gate Array):** Programovatelné hradlové pole umožňující vytvořit libovolný digitální obvod přímo na hardwarové úrovni s nulovou programovou latencí.
> Programovatelné hradlové pole. In: _Wikipedia: otevřená encyklopedie_ [online]. St. Petersburg (Florida): Wikimedia Foundation, 2005, poslední editace 10. 1. 2024 [cit. 2026-09-14]. Dostupné z: [https://cs.wikipedia.org/wiki/Programovateln%C3%A9_hradlov%C3%A9_pole](https://cs.wikipedia.org/wiki/Programovateln%C3%A9_hradlov%C3%A9_pole)
> Systém na čipu. In: _Wikipedia: otevřená encyklopedie_ [online]. St. Petersburg (Florida): Wikimedia Foundation, 2007, poslední editace 7. 6. 2024 [cit. 2026-09-14]. Dostupné z: [https://cs.wikipedia.org/wiki/Syst%C3%A9m_na_%C4%8Dipu](https://cs.wikipedia.org/wiki/Syst%C3%A9m_na_%C4%8Dipu)
>Digitální signálový procesor. In: _Wikipedia: otevřená encyklopedie_ [online]. St. Petersburg (Florida): Wikimedia Foundation, 2006, poslední editace 28. 2. 2026 [cit. 2026-09-14]. Dostupné z: [https://cs.wikipedia.org/wiki/Digit%C3%A1ln%C3%AD_sign%C3%A1lov%C3%BD_procesor](https://cs.wikipedia.org/wiki/Digit%C3%A1ln%C3%AD_sign%C3%A1lov%C3%BD_procesor)

<details>
<summary> :bulb: Tip k doplnění tabulky: </summary>
<p>Uvědomte si zásadní rozdíl: U MCU je program nahrán přímo ve vnitřní paměti Flash procesoru a startuje okamžitě po zapnutí (desítky milisekund). U MPU a iPC systém nejprve zavádí operační systém z disku/SD karty do paměti RAM (sekundy až desítky sekund).</p>
</details>

:star2: **Bonusová otázka k úloze 1:**

Proč se u bezpečnostních aplikací v letectví nebo jaderné energetice stále upřednostňují jednoduché mikrořadiče nebo FPGA před moderními vícejádrovými procesory s gigabajty RAM?

|Důvode je determinismus, předvídatelnost a certifikovatelnost. Vícejádrové procesory s velkou RAM a složitými mezipaměťmi využívají predikci skoků, sdílené sběrnice a dynamické plánování úloh. To způsobuje že doba vykonání instrukcí není zcela konstantní, což je pro bezpečnostní kritické systémy nepřípustné. Jednoduché MCU nebo FPGA umožňují exaktně dokázat a verifikovat každý takt procesoru a stav hardwaru.|



---

### 2. Parametry, paměti a provozní odolnost (IP krytí)
*Časová dotace: max. 15 minut | Úvodní úloha

1. **Typy pamětí:**
   - Jaký je zásadní rozdíl mezi pamětí **RAM**, **Flash** a **EEPROM** v mikrokontroléru/PLC z hlediska uchování dat po odpojení napájení a rychlosti zápisu?


   
RAM (Volatilní paměť): Data se po odpojení napájení ztratí. Je extrémně rychlá pro čtení i zápis, slouží pro běh programu, zásobník a aktuální proměnné.

Flash (Non-volatilní paměť): Data uchovává i bez napájení. Používá se pro uložení samotného firmwaru/programu. Zápis je pomalejší a má omezený počet cyklů přepsání (blokové mazání).

EEPROM (Non-volatilní paměť): Data uchovává bez napájení. Slouží k ukládání konfiguračních parametrů, kalibrací a stavů, které se mění za provozu. Umožňuje zápis po jednotlivých bytech a snese vyšší počet přepisů než Flash, ale má menší kapacitu.




2. **Reálný čas a determinismus:**
   - Proč pro řízení rychlého technologického děje (např. reakce na nouzové zastavení do 5 ms) použijeme spíše **MCU / PLC** než běžný operační systém na **MPU** (např. Raspberry Pi s OS Linux)?


  
Běžný operační systém na MPU (např. Linux bez RT patchů) je multitaskingový a spravuje mnoho procesů na pozadí. Nemá garantovaný čas reakce (non-real-time), takže může dojít k zpoždění (latenci) v řádu desítek až stovek milisekund kvůli plánovači úloh. MCU nebo PLC pracují deterministicky (cyklicky s pevným časem odezvy nebo s reálným operačním systémem RTOS), což zaručuje, že požadavek na nouzové zastavení bude zpracován okamžitě a v garantovaném čase.




3. **Odolnost a IP krytí:**
   - Dešifrujte označení **IP68** (co přesně znamená první číslice 6 a druhá číslice 8).
   - Jaké minimální krytí IP musí mít zařízení určené pro instalaci venku pod přístřeškem, kde hrozí stříkající voda a prach?
   - Jak se liší konstrukce běžného kancelářského PC od **průmyslového PC (iPC)** (např. z hlediska chlazení, napájení, vibrací a konektorů)?




První číslice (6): Úplná ochrana před vniknutím prachu (prachotěsné).

Druhá číslice (8): Ochrana proti potopení – zařízení je chráněno při trvalém ponoření do vody za podmínek stanovených výrobcem.

Minimální krytí IP venku pod přístřeškem (stříkající voda a prach):

Minimálně IP54 (ochrana proti prachu a stříkající vodě ze všech směrů). Pro vyšší jistotu se v průmyslu často volí IP65.

Rozdíl mezi kancelářským PC a průmyslovým PC (iPC):

Chlazení: Kancelářské PC spoléhá na ventilátory (nasává prach); iPC má pasivní chlazení (masivní hliníková žebra) nebo uzavřenou skříň s prachovými filtry.

Napájení: iPC mívá průmyslové zdroje s širším rozsahem vstupního napětí (např. stabilních 24 V DC) a galvanickým oddělením.

Vibrace: iPC používá SSD disky místo mechanických pevných disků a má pevně uchycené desky plošných spojů odolné vůči otřesům ve výrobě.

Konektory: iPC disponuje robustními šroubovacími konektory (M12, D-SUB) a průmyslovými rozhraními (RS485, galvanicky oddělené Ethernetové porty).




---
