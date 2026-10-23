# Changelog

## 1.2.0 — 2026-10-23

### Nuova funzionalità: spettri di risposta di progetto S_d(T)

- Nuovo pannello **"Spettri di risposta di progetto S_d(T)"** con grafico sovrapposto a quello elastico (elastici tratteggiati, di progetto in continuo, componenti orizzontale e verticale) e tabella di sintesi (`q`, `S_d(0)`, plateau, `S_d` a 1 s e 2 s, verifica del minimo).
- **SLO (§3.2.3.4)**: lo spettro di progetto coincide con l'elastico, per entrambe le componenti → `q` forzato a 1,0 anche per la verticale.
- **SLD / SLV / SLC (§3.2.3.5)**: applicata la sostituzione `η → 1/q` nelle formule [3.2.2] e [3.2.8]. Nel primo tratto `(T < T_B)` la riduzione **non** è una semplice divisione per `q`, perché `η` compare anche nel termine correttivo: `S_d(T) = a_g·S·(1/q)·F_0·[T/T_B + (q/F_0)(1 − T/T_B)]`.
- Applicato il minimo **`S_d(T) ≥ 0,2·a_g`** previsto dalla norma, con indicazione in tabella quando governa.
- Nuovo campo **fattore di comportamento `q`** (1–6, default 1,5) con richiamo ai limiti della Tab. 7.3.I; componente verticale con `q = 1,5` (1,0 per SLO).
- `verifica_numeri.py` esteso: verifica dei valori di `S_d` contro il calcolo manuale e del comportamento di SLO.

## 1.1.0 — 2026-10-23

Revisione tecnica di conformità alle NTC 2018 e correzione di anomalie emerse dai test.

### Correzioni normative

- **Isole (Allegato B, Tabella 2)** — aggiunta la tabella completa dei valori `a_g`, `F_0`, `T*_C` per i 5 gruppi di isole (Arcipelago Toscano, Egadi, Pantelleria, Sardegna, Pelagie, Ponza; Ventotene/S. Stefano; Ustica/Tremiti; Alicudi/Filicudi; Panarea/Stromboli/Lipari/Vulcano/Salina) con riconoscimento geografico puntuale (Sardegna per area rettangolare a sud del Capo Pertusato; le altre isole per centro + raggio, per non attribuire i valori tabellari alla costa antistante). Prima, per questi siti, l'app interpolava i nodi della costa: Sardegna −12,5 % su `a_g` e −13,1 % su `F_0`, Ustica −30,8 % su `a_g`.
- **Spettro elastico verticale (§3.2.3.2.2, Tab. 3.2.VI)** — corretto l'uso del coefficiente di amplificazione: `S_S = 1` per tutte le categorie e `S_T` da Tab. 3.2.V. Prima lo spettro verticale era sovrastimato del +20 % (cat. B) ÷ +48 % (cat. D) e sottostimato del 29 % con topografia T4.
- **Coefficiente `β_s` (Tab. 7.11.I)** — aggiunta la fascia `a_g > 0,4 g` (0,36 per cat. A, 0,32 per B–C–D–E): per `a_g` = 0,625 g il `k_h` passa da 0,175 a 0,200 (+12,5 %).
- **Liquefazione (§7.11.3.4.2)** — sostituita l'euristica con i criteri di esclusione **1** (`a_max` < 0,1 g) e **2** (falda > **15 m**, prima era 5 m, valore non previsto dalla norma). Aggiunta la dichiarazione esplicita che i criteri 3 e 4 richiedono indagini dirette.

### Affidabilità del dato

- **Punti fuori dal reticolo** — controllo della distanza dal nodo più vicino: oltre 15 km compare un avviso bloccante in pagina e nel PDF.
- **`T_R` fuori dall'intervallo 30–2475 anni** — il troncamento (previsto dall'Allegato A, nota 1) ora è segnalato con i valori richiesti.
- **Reticolo non caricato** — il calcolo è **disattivato** (pulsante disabilitato, risultati azzerati). Prima veniva usato un reticolo di riserva a 3 nodi che, per qualunque punto d'Italia, restituiva i valori dell'Aquila.
- **`F_0`** non più limitato a 3,2 (valore che la norma non prevede): il caricamento del reticolo ufficiale contiene nodi fino a 3,25.

### Tracciabilità del report

- Intestazione del report con **sito/commessa, data e ora, versione dell'applicativo, coordinate, fonte dei parametri** (reticolo o Tabella 2) e **ID e distanza dei 4 nodi interpolati**.
- Versione dell'applicativo riportata anche a piè di pagina.
- Campo libero "Nome sito / commessa".

### Disaggregazione

- Badge **"scenario illustrativo"** accanto al titolo del pannello.
- Avvertenza "non è un dato INGV" **spostata fuori** dal contenitore comprimibile (prima era visibile solo dopo aver espanso la matrice, e quindi mai a schermo).
- Titolo della sezione cambiato da "Terremoto dominante" a "Scenario dominante (illustrativo)".

### Interfaccia

- Corretto l'overflow orizzontale su schermi piccoli (392 px su viewport 375 px): `grid-template-columns: minmax(0,1fr)`, `min-width:0` su colonne e campi, tabella comprimibile sotto 560 px.

### Script di conversione

- `convert_grid.py`: corretto il ramo **CSV/TXT**, che falliva sul file ufficiale (`ValueError: Impossibile determinare l'ordine LAT/LON`). Ora gestisce campi fra virgolette, colonna `OBJECTID` anteposta, intestazioni su due righe, delimitatori `,`/`;`/tab e virgola decimale.
- `--decimals-val` portato da 3 a 4 decimali (la fonte ufficiale ne ha 4–5 in g/10).
- Aggiunti controlli su ID duplicati, numero di valori per `T_R` e numero di nodi.

### Documentazione

- README riscritto: comando di clone corretto (`GeoHub-Parametri-sismici`, prima puntava a un repository inesistente), metodo di calcolo con riferimenti puntuali alle norme, sezione **Limiti e perimetro d'uso**, nota **ED50/WGS84**, fonte dati con URL, tabella dei **valori di riferimento** per il collaudo.
- Aggiunto `verifica_numeri.py`: verifica automatica dei valori di riferimento, utile come test di non regressione.

### Note

- Nessuna modifica al formato di `grid-data.json`, che resta compatibile con le versioni precedenti.
- Il nome dell'applicazione (SismaSite) e quello del repository (GeoHub-Parametri-sismici) restano distinti: valutare se uniformarli.
