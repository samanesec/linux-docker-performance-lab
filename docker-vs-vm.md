# Docker vs. VM – Sammenlignende Lasttest med Nginx

Dette dokumentet sammenligner ytelsen til Nginx kjørt direkte på VM mot Nginx kjørt i en Docker-container, ved bruk av identiske lasttestparametere.

---

## Testmiljø

- **OS:** Ubuntu (VirtualBox)
- **CPU:** 1 kjerne — Intel Core i7-1355U
- **Verktøy:** ApacheBench (`ab`), `htop`, `docker`

> Merk: Systemet kjører med én CPU-kjerne, noe som betyr at alle prosesser konkurrerer om samme ressurs. Dette forsterker effekten av høy belastning.

---

## Oppsett

### 1. Verifiser Docker

```bash
sudo systemctl status docker
```

<img width="507" height="322" alt="image" src="https://github.com/user-attachments/assets/cf3a327a-9352-4a4d-9b69-bc87a16c4fe8" />


### 2. Last ned Nginx-image

```bash
sudo docker pull nginx
```

<img width="480" height="201" alt="image" src="https://github.com/user-attachments/assets/e793bbf0-222c-48c7-9ca5-a443f162e19f" />


### 3. Start Nginx-container

```bash
sudo docker run --name nginx-container -d -p 80:80 nginx
```

<img width="771" height="72" alt="image" src="https://github.com/user-attachments/assets/14239f22-1d84-43a1-a1b4-daf8bdd28a61" />


### 4. Verifiser at containeren kjører

```bash
sudo docker ps
```

<img width="940" height="74" alt="image" src="https://github.com/user-attachments/assets/f03947c9-79fc-4735-99e1-c9e39fd96f4e" />


| Container ID | Image | Ports | Names |
|---|---|---|---|
| 80c9310aed44 | nginx | 0.0.0.0:80->80/tcp | nginx-container |

---

## Lasttest

Samme kommando ble brukt som i VM-testen for direkte sammenligning:

```bash
ab -n 100000 -c 100 http://127.0.0.1/
```

| Parameter | Verdi | Beskrivelse |
|---|---|---|
| `-n 100000` | 100 000 | Totalt antall forespørsler |
| `-c 100` | 100 | Antall samtidige forespørsler |

<img width="1014" height="361" alt="image" src="https://github.com/user-attachments/assets/17ca58b9-89e4-41a8-a358-20c76d9ea381" />


---

## Resultater

| Parameter | VM | Docker |
|---|---|---|
| Nginx-versjon | nginx/1.27.3 | nginx/1.18.0 |
| Høyeste CPU-belastning | ~52% | ~44% |
| Load average (topp) | 0.95 | 3.22 |
| Lengste responstid | 502 ms | 847 ms |
| Antall feil | 0 | 0 |

---

## Analyse

### CPU-prosent vs. load average
CPU-prosenten alene er ikke det beste målet i denne testen. Load average gir et mer fullstendig bilde:

- **VM – load average 0.95:** Systemet håndterte lasten komfortabelt. En verdi under 1.0 på et enkeltkernesystem betyr at det ikke var kø av ventende prosesser.
- **Docker – load average 3.22:** Systemet var under betydelig press. En verdi på 3.22 på ett enkelt kjerne betyr at det til enhver tid var over tre prosesser i kø. Systemet klarte ikke å holde opp med lasten.

Docker skapte altså mer totalbelastning på systemet, selv om CPU-prosenten isolert sett ser lignende ut.

### Responstid
Den lengste responstiden var høyere med Docker (847 ms vs. 502 ms), noe som støtter at Docker-miljøet var under mer press under testen.

---

## Begrensninger ved testen

1. **Ulike Nginx-versjoner** — VM kjørte 1.27.3, Docker kjørte 1.18.0. Versjonsforskjeller kan påvirke ytelse og konfigurasjon.
2. **Samme maskin** — begge tester kjørte på samme VM, noe som kan påvirke resultatene.
3. **Enkeltkjerne-miljø** — med kun én CPU-kjerne forsterkes effekten av container-overhead betydelig sammenlignet med et flerkjernesystem.

En mer kontrollert test ville kjørt samme Nginx-versjon i begge miljøer på dedikert hardware med flere kjerner.

---

## Konklusjon

Load average er et bedre mål enn CPU-prosent i denne konteksten. Resultatene viser at Docker skapte mer systempress enn VM under samme lasttest, noe som samsvarer med forventet oppførsel: Docker legger til et nettverkslag og isolasjonslag som øker overhead, særlig merkbart på et enkeltkernesystem.

---

## Verktøy brukt

| Verktøy | Formål |
|---|---|
| `docker` | Containerisering og kjøring av Nginx |
| `ab` (ApacheBench) | Generere HTTP-last |
| `htop` | Overvåkning av CPU-belastning og load average |
| `systemctl` | Verifisere Docker- og Nginx-tjenester |
