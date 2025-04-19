# Home Security Alarm System

Jednoduchý Arduino projekt, který detekuje pohyb pomocí PIR senzoru a v případě narušení odesílá upozornění SMS zprávou přes GSM modul (SIM800L) nebo pomocí LoRa.

## Obsah

- [Přehled](#přehled)
- [Funkce](#funkce)
- [Komponenty](#komponenty)
- [Hardwarové zapojení](#hardwarové-zapojení)
- [Schéma zapojení](#schéma-zapojení)
- [Instalace softwaru](#instalace-softwaru)
- [Konfigurace](#konfigurace)
- [Použití](#použití)
- [Řešení problémů](#řešení-problémů)
- [Možná vylepšení](#možná-vylepšení)
- [Licence](#licence)

---

## Přehled

Tento projekt slouží jako základ domácího alarmu. Při detekci pohybu se spustí siréna (bzučák) a okamžitě odešle SMS na přednastavené číslo nebo vysílá zprávu přes LoRa.

## Funkce

- Detekce pohybu PIR senzorem (HC-SR501)
- Odeslání SMS upozornění přes GSM/GPRS modul (SIM800L) **nebo** bezdrátová zpráva přes LoRa
- Siréna (bzučák) a LED indikace stavu alarmu
- Tlačítko pro deaktivaci alarmu
- Nízkopříkonový režim (volitelné)

## Komponenty

| Komponenta                  | Počet | Poznámka                                   |
|-----------------------------|-------|--------------------------------------------|
| Arduino Uno (nebo kompatibilní) | 1     |                                            |
| PIR senzor HC-SR501         | 1     | Detekce pohybu                             |
| GSM/GPRS modul SIM800L      | 1     | Pro odesílání SMS                          |
| **nebo** LoRa modul (Ra-02) | 1     | Pro bezdrátovou komunikaci (alternativa)   |
| Bzučák / siréna             | 1     | Akustická signalizace                      |
| Tlačítko (push button)      | 1     | Pro deaktivaci alarmu                      |
| LED diody (červená, zelená) | 2     | Indikace stavu (aktivní/deaktivní)         |
| Rezistory (220 Ω)           | 2     | Pro LED diody                              |
| Propojovací vodiče          | –     |                                            |
| Napájecí zdroj (5V a 12V)   | 1     | 12V pro GSM modul, 5V pro Arduino          |

## Hardwarové zapojení

1. **PIR senzor**
   - VCC → 5V
   - GND → GND
   - OUT → Digitální pin D2
2. **GSM modul (SIM800L)**
   - VCC → 12V externí zdroj (ne přímo z 5V Arduino)
   - GND → GND (sdílené s Arduinem)
   - TX → RX (D10 pomocí SoftwareSerial)
   - RX → TX (D11 pomocí SoftwareSerial)
3. **Bzučák / siréna**
   - Pozitivní vodič → Digitální pin D3
   - Negativní vodič → GND
4. **LED diody**
   - Červená LED: anoda → D4 (přes rezistor 220 Ω) → katoda → GND
   - Zelená LED: anoda → D5 (přes rezistor 220 Ω) → katoda → GND
5. **Tlačítko**
   - Jeden konec → D6 s interním pull-down
   - Druhý konec → 5V

## Schéma zapojení

Do složky `hardware/` nahrajte PDF nebo PNG se schématem zapojení (např. `hardware/wiring_diagram.png`).

## Instalace softwaru

1. Stáhněte si a nainstalujte Arduino IDE (verze >=1.8.13).
2. Nainstalujte knihovny:
   - `SoftwareSerial`
   - `TinyGSM`
   - `LowPower` (volitelné, pro úsporný režim)
3. Otevřete `src/AlarmSystem.ino` v Arduino IDE.

## Konfigurace

V souboru `src/config.h` upravte:

```c
// PINY
#define PIR_PIN        2
#define BUZZER_PIN     3
#define LED_RED_PIN    4
#define LED_GREEN_PIN  5
#define BTN_PIN        6

// GSM nastavení
#define SIM_RX_PIN     10
#define SIM_TX_PIN     11
#define PHONE_NUMBER   "+420123456789"
#define SMS_APN        "internet"
#define SMS_USER       ""
#define SMS_PASS       ""
```

## Použití

1. Nahrajte kód do Arduino desky.
2. Zapněte napájení (5V pro Arduino, 12V pro GSM modul).
3. Alarm se aktivuje po zahřátí PIR senzoru (~30 s).
4. Při detekci pohybu:
   - Svítí červená LED a spustí se bzučák.
   - Odesílá se SMS na zadané číslo.
5. Stiskem tlačítka [BTN_PIN] alarm deaktivujete (zelená LED).

## Řešení problémů

- **Žádná SMS**: Zkontrolujte signál operátora, SIM kartu, APN, napájení GSM modulu.
- **Falešné poplachy**: Upravte citlivost PIR senzoru (trimr na modulu).
- **Bzučák nehraje**: Ověřte zapojení a dostatečné napájení.

## Možná vylepšení

- Přidání RTC modulu (DS3231) pro časové razítkování.
- Automatické arming/disarming dle času.
- Uložení logu poplachů na SD kartu.
- Napájení z LiPo baterie s nabíjecím modulem.

## Licence

MIT © [Vít Šebestýen]
