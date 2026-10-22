# Dati di esempio per la Rete dei rimontaggi

Tre dataset sintetici, ciascuno in due formati con gli stessi identici dati: `.csv` (colonne est, nord, quota) e `.geojson` (punti con la quota come terza coordinata). Caricati nello strumento danno gli stessi risultati.

Colonne: `RR` (ID frammento), `est`, `nord`, `quota`, `US`, `_Attacca_con`, `_pertinente_a`, `_Vaso_ID`.
I dati sono puliti: nessun avviso nella scheda Controlli. Per mostrare i controlli si usa l'esempio interno allo strumento, che contiene errori messi apposta.

Ogni dataset mostra una configurazione diversa degli attacchi.

## 1. Deposito stratificato

`1_deposito_stratificato` · 84 frammenti, 17 vasi, 5 US (101-105), 60 attacchi

Il caso "sano": vasi rotti in posto, ognuno dentro la sua US.

| Dove guardare | Che cosa si vede |
|---|---|
| Sintesi | 7% di attacchi tra US diverse (4 su 60); distanza mediana 24 cm |
| Matrice US | Quasi tutto sulla diagonale |
| Palinsesto | 56 attacchi nella stessa US, 4 tra US vicine in quota, nessun salto lungo; 5 insiemi, modularità 0,73 |
| Concentrazioni | 4 concentrazioni, 95% di attacchi interni (57 su 60) contro il 29% atteso se fossero distribuiti a caso |

Da far notare: il modello unisce le US 101 e 102 in una sola concentrazione perché sono vicine in quota, ma i rimontaggi le tengono distinte. La concentrazione è un raggruppamento statistico, non una fase cronologica, e non sostituisce l'US.

## 2. Fossa che taglia le US

`2_fossa_che_taglia` · 87 frammenti, 20 vasi, 4 strati (201-204) e il riempimento di una fossa (US 210), 60 attacchi

Una fossa taglia gli strati 203 e 204. Cinque vasi di quegli strati hanno parte dei frammenti in posto, sul bordo del taglio, e parte nel riempimento.

| Dove guardare | Che cosa si vede |
|---|---|
| Sintesi | 17% di attacchi tra US diverse (10 su 60); differenza di quota massima 50 cm |
| Pianta, colore US | Gli attacchi tra US diverse stanno tutti attorno a un punto (est 12, nord 22) |
| Palinsesto | Nessun attacco tra US vicine in quota, 10 attacchi che saltano due o più posizioni nell'ordine di quota; la US 210 ha apertura 48% |
| Matrice US | 210 legata solo a 203 e 204 |

Da far notare: lo strumento ordina la US 210 tra la 201 e la 202 per quota mediana, ma stratigraficamente il riempimento è posteriore alle US che taglia. È il caso in cui l'ordine di quota non è la sequenza stratigrafica.

## 3. Area d'uso e scarico

`3_area_uso_e_scarico` · 76 frammenti, 17 vasi, 3 US (301 piano d'uso, 305 scarico, 302 strato più antico), 55 attacchi

Sei vasi hanno i frammenti piccoli rimasti nell'area d'uso e i pezzi grandi buttati in uno scarico a circa 5 metri.

| Dove guardare | Che cosa si vede |
|---|---|
| Pianta | Fascio di linee lunghe tra due concentrazioni |
| Distanze | Due picchi: mediana 31 cm, ma massimo 5,7 m |
| Matrice US | 13 attacchi tra 301 e 305; la 302 non ha attacchi con le altre |
| Concentrazioni | Le due aree finiscono in concentrazioni diverse, ma 13 attacchi le collegano |

Da far notare: qui l'attacco lungo non è un disturbo. Area d'uso e scarico appartengono alla stessa occupazione, come le unità d'abitazione di Pincevent. La differenza di quota resta piccola (mediana 3 cm): lo spostamento è solo laterale.

## 4. Dati da correggere

`4_dati_da_correggere.csv` · solo CSV · è il dataset 1 con alcuni errori messi apposta, per mostrare il correttore (scheda Controlli, riquadro Correzioni proposte).

| Errore inserito | Frammento | Che cosa propone lo strumento |
|---|---|---|
| Riga copiata due volte | 1083 | Sicura: tenere una sola copia |
| Frammento che attacca con sé stesso | 1031 | Sicura: togliere l'auto-riferimento |
| Est e nord scambiati | 1039 | Sicura: scambiarli di nuovo |
| Quota con una cifra sbagliata (75.074) | 1067 | Da verificare: 79.074 |
| Stesso vaso scritto in due modi (V5 e V05) | 1091 | Da verificare: unificare in V05 |
| Attacco verso un frammento che non esiste (1999) | 1007 | Nessuna correzione automatica: va sistemato a mano nella scheda Dati |

## Il matrix di Harris: come crearlo e importarlo

Il matrix è facoltativo. Con il matrix gli attacchi tra US vengono letti sulla **sequenza stratigrafica vera** e non più sull'ordine di quota. Le coordinate x, y, z dei frammenti non cambiano: il matrix aggiunge solo i rapporti tra le US.

### Come si crea

Un file CSV (da Excel, LibreOffice o esportato dal database di scavo) con **una riga per rapporto stratigrafico** e tre colonne: US, rapporto, US.

```
US;rapporto;US
6;copre;16
16;copre;17
17;copre;20
20;copre;21
20;copre;33
20;copre;36
36;riempie;34
34;taglia;21
34;taglia;33
21;si appoggia a;30
33;si appoggia a;30
```

- Si legge da sinistra a destra: "6 copre 16", "36 riempie 34", "34 taglia 21".
- Basta scrivere ogni rapporto una volta; anche le forme inverse vanno bene ("16 coperto da 6").
- Rapporti riconosciuti:

| Rapporto | Forma inversa | Significato per la sequenza |
|---|---|---|
| copre | coperto da | la prima US è più recente |
| taglia | tagliato da | la prima US è più recente |
| riempie | riempito da | la prima US è più recente |
| si appoggia a | gli si appoggia | la prima US è più recente |
| posteriore a | anteriore a | la prima US è più recente |
| uguale a | | stessa posizione nella sequenza (contemporanee) |
| si lega a | | stessa posizione nella sequenza (contemporanee) |

  Sono riconosciute anche le forme inglesi (covers, cuts, fills, abuts, same as, bonds with).
- I nomi delle colonne sono liberi; il separatore (punto e virgola, virgola, tabulazione) viene riconosciuto da solo.
- Le US possono essere scritte come `20` o `US 20`; "20 base" e "20 tetto" valgono come US 20.
- Possono comparire US senza frammenti, per esempio tagli e strutture (nell'esempio la fossa US 34 e il muro US 30).

### Come si importa

1. Carica prima i dati dei frammenti.
2. Nel pannello a sinistra, riquadro **Matrix di Harris**, trascina il CSV o fai clic su **Carica il matrix**. Va bene anche trascinarlo nel riquadro Dati: lo strumento riconosce il matrix dalla colonna "rapporto".
3. Apri la scheda **Matrix**: il diagramma mostra le US dalla più recente alla più antica, con gli attacchi disegnati come archi colorati.

Con **Scarica il modello** si ottiene un CSV di partenza. Il matrix si salva insieme al progetto. Se carichi un altro file di frammenti, il matrix viene tolto, perché le US sono diverse.

### Che cosa calcola

Ogni attacco tra due US viene classificato:

- nella stessa US;
- tra US contemporanee ("uguale a", "si lega a");
- tra US in rapporto diretto (un passaggio nel matrix);
- tra US a due, tre o più passaggi;
- tra US su **rami diversi**, che il matrix non mette in sequenza: possono essere contemporanee, o la stessa unità divisa in scavo.

Il matrix viene anche controllato: righe non riconosciute, cicli (A copre B e B copre A), US dichiarate contemporanee e insieme in sequenza, US con frammenti che non compaiono nel matrix.

### Matrix di esempio

| File | Da usare con | Che cosa mostra |
|---|---|---|
| `0_esempio_lezione_matrix.csv` | l'esempio interno allo strumento (si carica da solo con l'esempio) | La US 36 riempie una fossa (US 34) che taglia le US 21 e 33: è più bassa in quota ma più recente. Le US 21 e 33 si appoggiano a un muro (US 30) e stanno su rami diversi: i loro 6 attacchi fanno pensare che siano contemporanee |
| `1_deposito_stratificato_matrix.csv` | `1_deposito_stratificato` e `4_dati_da_correggere` | Una sequenza semplice: quasi tutti gli attacchi nella stessa US |
| `2_fossa_che_taglia_matrix.csv` | `2_fossa_che_taglia` | La fossa US 211 (senza frammenti) taglia 202 e 203 ed è riempita da 210: gli attacchi del riempimento sono a 2 o 3 passaggi |
| `3_area_uso_e_scarico_matrix.csv` | `3_area_uso_e_scarico` | Piano d'uso e scarico legati ("si lega a"): i loro attacchi risultano tra US contemporanee |

## Come deve essere un file perché venga accettato

Lo strumento legge CSV (anche `.txt` e `.tsv`) e GeoJSON di punti. Non serve installare nulla: basta il browser.

### CSV

```
RR,est,nord,quota,US,_Attacca_con,_pertinente_a,_Vaso_ID
1003,201.922,301.548,79.628,101,,,V01
1007,201.717,301.679,79.642,101,1003,,V01
```

- La prima riga contiene i nomi delle colonne.
- Separatore: virgola, punto e virgola, tabulazione o barra verticale, riconosciuto da solo.
- Decimali con il punto o con la virgola (con la virgola serve un separatore diverso, per esempio il punto e virgola).
- Codifica UTF-8; i file di Excel in Windows-1252 vengono letti lo stesso.

### GeoJSON

```json
{ "type": "FeatureCollection", "features": [
  { "type": "Feature",
    "geometry": { "type": "Point", "coordinates": [201.717, 301.679, 79.642] },
    "properties": { "RR": "1007", "US": "101", "_Attacca_con": "1003", "_Vaso_ID": "V01" } }
] }
```

- Una `FeatureCollection` di punti (`Point`; di un `MultiPoint` si usa il primo punto).
- Est, nord e quota vengono dalla geometria; se il punto è bidimensionale la quota può stare in un attributo.
- Gli attributi (`properties`) diventano le colonne.

### Regole comuni

| Campo | Obbligatorio | Nomi riconosciuti da soli | Note |
|---|---|---|---|
| ID frammento | sì | `RR`, `frammento`, `id` | Univoco. Uno stesso ID in due posizioni diverse è segnalato come errore |
| Attacca con | sì | `attacca_con`, `refit`, `rimontaggio` | Uno o più ID separati da punto e virgola, virgola, barra o spazio. Vuoto, `0`, `?`, `nd` = nessun attacco |
| X / Est | sì | `x`, `est`, `east`, `easting` | In metri |
| Y / Nord | sì | `y`, `nord`, `north`, `northing` | In metri |
| Quota | no | `z`, `quota`, `elev` | Serve per proiezioni verticali, 3D e concentrazioni |
| US | no | `US`, `contesto`, `strato`, `layer` | Serve per matrice US e palinsesto. "20 base" e "20 tetto" contano come US 20 |
| Pertinente a | no | `pertinente_a` | Stesse regole di "Attacca con" |
| Vaso | no | `vaso`, `vaso_id`, `vessel` | |

- Se le colonne hanno altri nomi si scelgono a mano nel pannello Campi: non serve rinominarle.
- Le coordinate devono essere in metri (griglia locale o sistema proiettato). Con latitudine e longitudine in gradi le distanze risultano sbagliate.
- Il legame basta scriverlo in una delle due righe: lo strumento lo rende simmetrico.
- Gli ID scritti in "Attacca con" devono esistere nella colonna dell'ID, altrimenti compaiono tra i Controlli.
