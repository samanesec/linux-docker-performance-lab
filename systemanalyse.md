# Systemanalyse – CPU-kartlegging og nettverksovervåkning

Dette dokumentet beskriver analyse av CPU-ressurser og nettverkstilstand i et Ubuntu-miljø ved bruk av innebygde Linux-verktøy.

---

## CPU-kartlegging med lscpu

For å kartlegge CPU-informasjon ble følgende kommando brukt:

```bash
lscpu
```

### Resultater

| Parameter | Verdi |
|---|---|
| Arkitektur | x86_64 |
| CPU-modell | 13th Gen Intel Core i7-1355U |
| Antall kjerner per socket | 1 |
| Tråder per kjerne | 1 |
| BogoMIPS | 5222.38 |
| Byteorden | Little Endian |

<img width="619" height="411" alt="image" src="https://github.com/user-attachments/assets/738874f7-bd0c-41b3-9faf-903024321e97" />

### Analyse
Systemet kjører med én kjerne og én tråd, noe som betyr at alle prosesser konkurrerer om samme CPU-ressurs. Dette er relevant for ytelsestesting, da høy CPU-belastning vil påvirke alle prosesser direkte.

---

## Nettverksanalyse med netstat

For å kartlegge åpne porter og nettverkstilstand ble følgende kommando brukt:

```bash
netstat -tuln
```

### Resultater

Kommandoen viser:
- **Proto** – protokoll i bruk (TCP/UDP)
- **Local Address** – IP-adresse og portnummer som lytter
- **Foreign Address** – ekstern adresse og port
- **State** – tilstand (LISTEN = åpen og klar for innkommende tilkoblinger)

<img width="467" height="390" alt="image" src="https://github.com/user-attachments/assets/588895b2-f712-47d2-b7b4-85dbc1cf3acc" /><img width="471" height="161" alt="image" src="https://github.com/user-attachments/assets/d85698bb-68f5-4115-9a71-73c41191cd8f" />


### Porter i LISTEN-tilstand

| Port | Protokoll | Beskrivelse |
|---|---|---|
| 80 | TCP | HTTP – netttrafikk |
| 443 | TCP | HTTPS – kryptert netttrafikk |
| 3306 | TCP | MySQL – databasetilkoblinger |

### Analyse
Flere porter var i LISTEN-tilstand, noe som indikerer aktive tjenester på systemet. Port 80 og 443 bekrefter at en webserver kjører, mens port 3306 viser at en MySQL-database er tilgjengelig. I et produksjonsmiljø bør alle unødvendige åpne porter lukkes for å redusere angrepsflaten.

---

## Verktøy brukt

| Verktøy | Formål |
|---|---|
| `lscpu` | Kartlegge CPU-arkitektur og kapasitet |
| `netstat -tuln` | Vise åpne porter og nettverkstilstand |

---

## Sikkerhetsvurdering

- Åpne porter bør gjennomgås jevnlig og unødvendige tjenester bør deaktiveres
- Port 3306 bør ikke eksponeres mot internett uten tilgangskontroll
- Bruk av `ss` som alternativ til `netstat` anbefales på nyere Linux-systemer. Dette er en litt eldre versjon
