# Come caricare il pacchetto su GitHub

Il contenuto di questa cartella **sostituisce** i file omonimi del repository
`geologiaintasca/GeoHub-Parametri-sismici`.

## File da caricare

| File | Azione | Note |
|---|---|---|
| `index.html` | **sostituisci** | app corretta e estesa (v1.2.0) |
| `convert_grid.py` | **sostituisci** | ramo CSV/TXT riparato |
| `README.md` | **sostituisci** | riscritto |
| `CHANGELOG.md` | **nuovo** | aggiungilo alla root |
| `verifica_numeri.py` | **nuovo** | test automatico (opzionale ma consigliato) |
| `grid-data.json` | **non toccare** | invariato (MD5 identico all'originale) |
| `LICENSE` | **non toccare** | invariato |

## Opzione A — Interfaccia web di GitHub (più semplice)

1. Apri <https://github.com/geologiaintasca/GeoHub-Parametri-sismici>.
2. Per ogni file da sostituire: clicca sul file → icona **matita** (*Edit*) → seleziona tutto (`Ctrl+A`) → incolla il contenuto del file nuovo → **Commit changes**.
   *In alternativa*: **Add file → Upload files**, trascina i file e conferma il commit (sovrascrive quelli con lo stesso nome).
3. Per i file nuovi: **Add file → Upload files**, trascina `CHANGELOG.md` e `verifica_numeri.py`, poi **Commit changes**.
4. Attendi 1–2 minuti: GitHub Pages ripubblica automaticamente la demo su
   <https://geologiaintasca.github.io/GeoHub-Parametri-sismici/>.
5. Verifica: apri la demo, controlla che in alto compaia `SismaSite v1.2.0 (2026-10-23)` e che sia presente il pannello **"Spettri di risposta di progetto S_d(T)"**.

## Opzione B — Git da terminale (consigliata, conserva la cronologia)

```bash
git clone https://github.com/geologiaintasca/GeoHub-Parametri-sismici.git
cd GeoHub-Parametri-sismici

# copia i file del pacchetto nella cartella (adattando il percorso)
cp /percorso/del/pacchetto/index.html .
cp /percorso/del/pacchetto/convert_grid.py .
cp /percorso/del/pacchetto/README.md .
cp /percorso/del/pacchetto/CHANGELOG.md .
cp /percorso/del/pacchetto/verifica_numeri.py .

git add -A
git commit -m "v1.2.0: conformità NTC 2018 (isole Tab. 2, spettro verticale, beta_s, liquefazione), spettri di progetto Sd(T), tracciabilità del report, fix CSV in convert_grid.py"
git push origin main
```

## Opzione C — Applicando le patch (se preferisci rivedere le modifiche prima)

Le patch unificate sono nella cartella `fix/` e si applicano su un clone pulito:

```bash
git clone https://github.com/geologiaintasca/GeoHub-Parametri-sismici.git
cd GeoHub-Parametri-sismici
git apply /percorso/fix/correzioni_v120.diff
git apply /percorso/fix/convert_grid.diff
git apply /percorso/fix/README.diff
cp /percorso/CHANGELOG.md /percorso/verifica_numeri.py .
git add -A && git commit -m "v1.2.0: correzioni NTC 2018 e spettri di progetto" && git push
```

Le patch sono state verificate: applicate su un clone del repository ricostruiscono
**esattamente** i file del pacchetto.

## Controllo finale consigliato (facoltativo)

```bash
pip install playwright && python3 -m playwright install chromium
python3 verifica_numeri.py     # deve terminare con "VERIFICA SUPERATA"
```

Lo script avvia da solo un server locale, apre l'app in un browser headless e confronta
i risultati con i valori di riferimento (7 località + spettro di progetto). Esce con
codice 0 solo se tutto torna: è utilizzabile anche in una GitHub Action.

## Dopo l'upload: due cose da valutare

1. **Nome**: repository `GeoHub-Parametri-sismici`, README "SismaSite", interfaccia "SismaSite". Conviene uniformare (o rinominare il repository in `SismaSite`, GitHub reindirizza i vecchi URL).
2. **Tag di versione**: `git tag -a v1.2.0 -m "Conformità NTC 2018 + spettri di progetto" && git push --tags`, così le relazioni possono citare una versione precisa.
