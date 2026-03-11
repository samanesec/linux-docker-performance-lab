# Linux & Docker – Ytelsesanalyse og Nettverksovervåkning

Dette prosjektet dokumenterer praktiske laboratorieøvelser innen Linux-systemanalyse, nettverksovervåkning og ytelsestesting med og uten Docker. Øvelsene ble gjennomført i et virtualisert Ubuntu-miljø med VirtualBox.

---

## Innhold i dette repoet

| Fil | Beskrivelse |
|---|---|
| `systemanalyse.md` | CPU-kartlegging og nettverksanalyse med Linux-verktøy |
| `nginx-lasttest.md` | Ytelsestesting av Nginx med ApacheBench og htop |
| `docker-vs-native.md` | Sammenlignende lasttest – Nginx med og uten Docker |

---

## Miljø

- **OS:** Ubuntu (VirtualBox)
- **Verktøy:** `lscpu`, `netstat`, `htop`, `ab` (ApacheBench), `docker`, `nginx`
- **Metode:** Manuell testing og analyse med skjermbilder og målinger

---

## Nøkkelfunn

| Test | CPU-belastning | Forespørsler per sekund |
|---|---|---|
| Nginx uten Docker | ~60% | 1 668 req/s |
| Nginx med Docker | 85–95% | 4 578 req/s |

Docker håndterte flere forespørsler per sekund, men til en høyere CPU-kostnad. Dette illustrerer avveiningen mellom gjennomstrømning og ressursbruk i containeriserte miljøer.

---

## Teknologier

`Linux` `Ubuntu` `Docker` `Nginx` `ApacheBench` `htop` `netstat` `lscpu` `VirtualBox`
