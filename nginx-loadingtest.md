# Nginx Lasttest – ApacheBench og htop

Dette dokumentet beskriver ytelsestesting av en Nginx-webserver ved bruk av ApacheBench og sanntidsovervåkning med htop i et Ubuntu-miljø.

---

## Oppsett

### Start og verifiser Nginx

```bash
sudo systemctl start nginx
sudo systemctl status nginx
```

<img width="661" height="318" alt="image" src="https://github.com/user-attachments/assets/388e1657-3f68-457e-8cb6-efa85b1c0d0b" />


Nginx ble bekreftet som aktiv og kjørende før testing startet.

---

## Lasttest med ApacheBench

To terminalvinduer ble satt opp parallelt:
- **Terminal 1** – sender forespørsler med ApacheBench
- **Terminal 2** – overvåker CPU og prosesser med htop

### Kommando

```bash
ab -n 100000 -c 100 http://127.0.0.1/
```

| Parameter | Verdi | Beskrivelse |
|---|---|---|
| `-n 100000` | 100 000 | Totalt antall forespørsler |
| `-c 100` | 100 | Antall samtidige forespørsler |
| `http://127.0.0.1/` | Localhost | Målserver |

<img width="940" height="329" alt="image" src="https://github.com/user-attachments/assets/a977af02-a46a-4cda-8644-cb4772635b60" />


---

## Resultater

| Parameter | Verdi |
|---|---|
| Høyeste CPU-belastning | ~52% |
| Load average (topp) | 0.95 |
| Lengste responstid | 502 ms |
| Gjennomsnittlig responstid | 59,932 ms |
| Testvarighet | 59,932 sekunder |
| Antall feil | 0 |

---

## Analyse

CPU-belastningen nådde ~52% under lasttesten. Belastningen kom fra to prosesser som kjørte parallelt:

1. **ApacheBench** – genererte høy mengde samtidige forespørsler
2. **Nginx** – behandlet forespørslene og returnerte svar

Load average på 0.95 viser at systemet håndterte lasten komfortabelt — ingen kø av ventende prosesser. Ingen feil ble registrert, noe som bekrefter at Nginx var stabil gjennom hele testen.

---

## Verktøy brukt

| Verktøy | Formål |
|---|---|
| `ab` (ApacheBench) | Generere HTTP-last mot webserver |
| `htop` | Sanntidsovervåkning av CPU og prosesser |
| `systemctl` | Starte og verifisere Nginx-tjenesten |
