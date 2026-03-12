# Linux & Docker – Ytelsesanalyse og Nettverksovervåkning

Dette prosjektet dokumenterer praktiske laboratorieøvelser innen Linux-systemanalyse, nettverksovervåkning og ytelsestesting både på host og i Docker-container. Øvelsene ble gjennomført i et virtualisert Ubuntu-miljø med VirtualBox.

---

## Innhold i dette repoet

| Fil | Beskrivelse |
|---|---|
| `systemanalyse.md` | CPU-kartlegging og nettverksanalyse med Linux-verktøy |
| `nginx-loadingtest.md` | Ytelsestesting av Nginx med ApacheBench og htop |
| `docker-vs-vm.md` | Sammenlignende lasttest – Nginx i Docker vs. Host |

---

## Testmiljø

- **OS:** Ubuntu (VirtualBox)
- **CPU:** 1 kjerne — Intel Core i7-1355U
- **Verktøy:** `lscpu`, `netstat`, `htop`, `ab` (ApacheBench), `docker`, `nginx`

---

## Nøkkelfunn

| Test | CPU-belastning | Load average | Lengste responstid |
|---|---|---|---|
| Nginx – Host | ~52% | 0.95 | 502 ms |
| Nginx – Docker | ~44% | 3.22 | 847 ms |

Load average er et bedre mål enn CPU-prosent i denne testen. En load average på 3.22 på ett enkelt kjerne betyr at systemet hadde en kø av ventende prosesser. Docker skapte mer totalbelastning selv om CPU-prosenten isolert sett var lavere.

---

## Teknologier

`Linux` `Ubuntu` `Docker` `Nginx` `ApacheBench` `htop` `netstat` `lscpu` `VirtualBox`
