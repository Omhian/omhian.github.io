# Heliophobia Press — sito

## Avvio

Installa Hugo (versione 0.146 o successiva), poi dentro questa cartella:

    hugo server -D

Apri http://localhost:1313 — si aggiorna da solo mentre scrivi.

Per generare il sito da pubblicare:

    hugo

Il risultato finisce in `public/`.

## Scrivere un articolo

    hugo new editoriali/nome-del-pezzo.md

Il file nasce in `content/editoriali/`. In cima c'è il front matter:

    ---
    title: "Titolo"
    date: 2026-09-16
    autore: "Nome"
    firma: "Testo della firma in fondo al pezzo"
    draft: true
    ---

Togli `draft: true` quando è pronto. Con `draft: true` il pezzo si
vede solo in locale (`hugo server -D`), non online.

## Immagini

Mettile in `static/img/`. Nel markdown:

    ![](/img/nomefile.jpg)

Occupano automaticamente tutta la larghezza del foglio bianco,
sbordando oltre i margini del testo.

## Cosa modificare, e dove

- **Colori, larghezze, misura del testo** → le variabili in cima a
  `static/css/main.css`. Sono tutte lì, prime venti righe.
- **Voci della barra verde** → le sezioni `[[menu.main]]` in `hugo.toml`.
- **Marchio in testata** → l'SVG dentro `layouts/baseof.html`.
- **Foto per la firma** → mettila in `static/img/` e aggiungi a `hugo.toml`:

      [params]
        foto = "/img/tuafoto.jpg"

## Struttura

    layouts/baseof.html   testata nera + barra verde + piede
    layouts/home.html     pagina iniziale
    layouts/page.html     singolo articolo
    layouts/section.html  elenco di una sezione
    static/css/main.css   tutto lo stile
    content/              i testi

## Nota sul font

La scritta PRESS nel marchio usa Lacquer, caricato da Google Fonts nel
`<head>`. Se un giorno vuoi che il sito non dipenda da servizi esterni,
va scaricato il file del font e servito da `static/`, oppure la scritta
va convertita in tracciati dentro l'SVG.

## La griglia a 80 colonne e 24 righe

Il corpo del testo e' monospaziato (Anonymous Pro) e largo ottanta
caratteri esatti. Il font si rimpicciolisce con lo schermo in modo che
le ottanta colonne ci stiano sempre, fino a un minimo leggibile.

La griglia non si vede, ma regge tutto: ogni margine verticale e' un
multiplo esatto di `--riga`. **Se aggiungi stili tuoi, rispetta questa
regola** o il ritmo verticale va fuori fase.

### Immagini

Occupano sempre ventiquattro righe, cioe' una schermata intera, e tutta
la larghezza del foglio bianco. Sono ritagliate al centro, non scalate:
il formato dell'originale non conta, l'ingombro sulla pagina e' sempre
lo stesso.

    ![](/img/foto.jpg)

Se l'originale e' molto alto o molto stretto, il ritaglio mangia
parecchio: conviene preparare le immagini gia' panoramiche, indicativamente
tre volte piu' larghe che alte.

### Separatore manuale

Dove vuoi dichiarare tu la fine di una schermata:

    {{</* schermata "fine scheda" */>}}

L'etichetta e' facoltativa.

### Le manopole

In cima a `main.css`:

    --corpo      dimensione del testo (si adatta allo schermo)
    --riga       altezza di una riga: tutto il ritmo dipende da questa
    --schermata  righe per schermata (24): e' anche l'altezza delle immagini
    --colonne    larghezza del testo (80ch = 80 caratteri)

## I tre inchiostri del foglio

    nero     #16160F   testo e titoli: la struttura
    indaco   #2B2A8C   grassetto: l'enfasi
    arancio  #FF4A1C   link: quello su cui si clicca

Tre segnali, tre funzioni. Se aggiungi un quarto colore al foglio,
verifica prima che stia dicendo una cosa che gli altri tre non dicono.

I titoli restano neri di proposito: sono gia' distinti da maiuscoletto,
spaziatura e aria sopra. Colorarli toglierebbe all'indaco il significato
di "qui c'e' un accento" per ridurlo a decorazione.

## Metterlo su GitHub

Il repo pubblico contiene i sorgenti, non il sito compilato: ci pensa
GitHub a ricostruirlo a ogni push, con il flusso in
`.github/workflows/pages.yml`.

    git init
    git add .
    git commit -m "Primo commit"
    git branch -M main
    git remote add origin https://github.com/TUONOME/NOMEREPO.git
    git push -u origin main

Poi, una volta sola, su GitHub:

1. Settings > Pages > Source: scegli **GitHub Actions** (non "Deploy from
   a branch"). Senza questo passaggio il flusso gira e fallisce in fondo.
2. Sempre in Settings > Pages, metti il dominio in "Custom domain" e
   salva. **Fallo prima di toccare il DNS**, non dopo: dichiararlo qui
   per primo impedisce a chiunque altro di rivendicare quel nome su
   Pages.
3. Solo adesso, su Porkbun, i record DNS. Poi torna qui e spunta
   "Enforce HTTPS" (compare dopo qualche minuto, quando Let's Encrypt
   ha emesso il certificato).

Il flusso prende l'indirizzo del sito da GitHub, non da `hugo.toml`:
finche' il dominio non e' attivo il sito funziona su nome.github.io, e
quando lo attivi passa al dominio da solo. Non devi cambiare niente.

### Le bozze

Se vuoi tenere un pezzo fuori dal sito senza spostarlo di repo, mettigli
nel front matter:

    draft: true

Hugo lo ignora in compilazione. Resta pero' visibile nel repo: se la
bozza deve essere davvero privata, quella e' la ragione per tenerla
nell'altro repo.

Per vederla in locale mentre ci lavori:

    hugo server -D

## Sfondo

Il nero del fondo porta una grana: `static/img/grana-piu.png`, un tassello
da 256 px ripetuto, 2,8 KB. E' un'immagine ferma, non un'animazione:
niente script e niente batteria.

La ripetizione non si vede, e il motivo e' proprio che e' debole. Una
giunta si nota solo se c'e' qualcosa di abbastanza marcato da
riconoscere; a questo contrasto non c'e'. Se un domani la vuoi piu'
presente, rigenera il tassello piu' acceso ma sappi che da li' in poi la
ripetizione comincia a vedersi.

Si vede solo nei margini laterali, dietro la testata e sotto il piede: il
foglio bianco e' opaco e copre il centro. Su telefono il foglio arriva ai
bordi e non si vede quasi niente.

## Interlinea

`--riga` e' `--corpo * 1.37`. Era 1.45.

Il corpo non e' fisso, e' `clamp(11px, 1.45vw, 17px)`: si restringe con lo
schermo perche' le 80 colonne ci stiano sempre. Per questo l'interlinea e'
un rapporto e non una misura in punti: togliere un punto fisso
significherebbe togliere il 5% sul desktop e il 9% su schermo stretto,
cioe' proprio dove serve piu' aria.

Sotto 1.37, su righe da 80 caratteri, l'occhio comincia a sbagliare rigo
quando torna a sinistra. Se un giorno vorrai davvero l'effetto colonna di
giornale, la leva non e' l'interlinea ma la larghezza: a 62 caratteri
un'interlinea stretta funziona. Costa pero' le 80 colonne, che sono il
motivo per cui il sito e' questo sito.

## Immagini

Tre disposizioni. In tutte e tre l'altezza e' un numero intero di righe,
cosi' il testo sotto resta in fase con la griglia di base.

**Schermata intera.** Una normale immagine markdown: `![](/img/x.jpg)`.
Larga quanto il foglio, alta 24 righe, ritagliata al centro. Prepara
immagini panoramiche, circa 3:1.

**Mezza schermata.** Larga uguale, alta 12 righe: taglia la pagina a meta'
invece di riempirla.

    {{< mezza src="/img/x.jpg" >}}Didascalia{{< /mezza >}}

**Al vivo con testo intorno.** Tocca il bordo del foglio da un lato, il
testo le scorre accanto con due righe di stacco.

    {{< lato src="/img/x.jpg" lato="destra" righe="12" >}}Didascalia{{< /lato >}}

    lato    sinistra | destra            (predefinito: destra)
    righe   altezza in righe di griglia  (predefinito: 12)
    largo   terzo                        (senza, e' meta' colonna)
    alt     testo alternativo            (senza, usa la didascalia)

La didascalia puo' andare a capo quanto vuole: ogni sua riga e' alta una
riga di griglia, quindi il conto torna comunque.

Sotto i 760 px le immagini al vivo tornano a tutta larghezza: su telefono
il testo accanto a un'immagine finirebbe in un filo di spazio illeggibile.

I titoli `h2` e `h3` ricominciano sempre sotto un'immagine, non le si
affiancano mai.
