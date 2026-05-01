# SPI FLASH programátor
Tento repozitář obsahuje souboru k návrhu SPI Flash programátoru.

## Struktura
- Výrobní soubory jsou v adresáři: programator_spi/production/programator_spi.zip
- Schéma je dostupné v adresáři: programator_spi/schema_pdf/programator_spi.pdf
# Návrhová rozhodnutí

## Patice pro DIP8

Původní záměr byl použít ZIF socket, ten se však pro pDIP8 pouzdro prakticky nevyrábí. Zvolil jsem proto přesnou patici z doporučených komponent. Klíčovými kritérii byl nízký odpor kontaktů a zlaté pokovení pinů. Životnost patice je dle dokumentace přibližně 500 cyklů, což by pro daný účel mělo být dostatečné. Pokud by se požadavky na počet cyklů zvýšily, bylo by nutné přejít na ZIF patici většího formátu.

## USB rozhraní

Nejprve jsem navrhl USB rozhraní. Použil jsem ESD ochranu z doporučených komponent. Zvažoval jsem ještě ECMF02-2AMX6, která nabízí common mode ochranu a je použita například v referenčním designu MB1801.

Dále jsem přidal proudovou ochranu — polyfuse. Maximální proud jsem stanovil takto: flash paměť odebírá při zápisu až 30 mA, MCU pak dle datasheetu až 150 mA. Zvolil jsem proto polyfuse 200 mA, která poskytuje rozumnou rezervu; v případě potřeby ji lze snadno vyměnit.

Dle specifikace jsem na linku D+ přidal pull-up rezistor 1k5Ω.

## MCU

S STM32 jsem dosud nenavrhoval, proto jsem si stáhl AN a postupoval podle ní. Jako první jsem řešil napájení — přidal jsem blokovací kondenzátory na příslušné piny. Oddělení napájení analogové části jsem neřešil, protože zde nemá smysl.

Vybral jsem krystal 8 MHz tak, aby byl splněn margin, a navrhl jsem kondenzátory s tím, že Cp uvažuji jako 5 pF. Margin jsem ověřil dle AN2867 — vychází gm = 26,4 mA/V, rezerva je dostatečná.

## Programovací rozhraní

Pokračoval jsem programovacím rozhraním. Zvolil jsem SWD s výstupem SWO, konektor Tag-Connect.

## Tlačítka

Přidal jsem tlačítko RESET, protože je vhodné ho mít vždy vyvedené, a tlačítko BOOT pro změnu bootovacího režimu, například pro DFU přes USB. Tlačítko ERASE ani FLASH není zmíněno v zadání, takže jsem je nepřidával — zbytečná komplexita.

## Indikace

Pro indikaci stavu jsem zvolil adresovatelnou LED, která funguje s 5V logikou. Teoreticky by byl potřeba level shifter, nicméně v praxi funguje spolehlivě i bez něj, zejména na malých vzdálenostech.

## Flash

K napájení jsem přidal 100 nF kondenzátor, SPI jsem připojil k MCU, /WP a /HOLD jsem vyvedl na VDD, protože ani jednu z funkcí nepotřebuji. Na piny sdílené s MCU jsem přidal ESD ochranu.

## Test pady
Přidávám testpady s dírou uprostřed, protože v nich líp drží probe osciloskopu a ve dvouvrstvé desce ničemu nevadí.

*Text Návrhová rozhodnutí byl upravenu pomocí generativní umělé inteligence, originál je dostupný v: original_notes/notes.txt*
