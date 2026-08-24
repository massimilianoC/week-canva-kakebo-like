# Farlo tuo

🇬🇧 [This guide in English](CUSTOMIZE.md)

Sta tutto in `index.html`. I punti da toccare sono due soltanto:

1. il **blocco `:root`** in cima allo `<style>` — colori, font, geometria;
2. il **corpo HTML** — le parole sul foglio.

Nessuna build. Salvi il file, ricarichi il browser, stampi.

> **Preferisci farlo fare a un agente AI?** Vai al [§8](#8-darlo-in-mano-a-un-agente-ai)
> per i prompt pronti. Fallo partire da [`../AGENTS.md`](../AGENTS.md): contiene
> i vincoli che tengono il foglio stampabile.

---

## 1. Farlo tuo in dieci minuti

Il foglio arriva con gli ambiti di vita dell'autore. Sostituiscili ed è il tuo
planner. Quattro modifiche, in ordine di importanza.

### Le quattro rotte — ⚠️ ogni nome compare due volte

È la modifica che va storta più spesso. Ogni nome di rotta è scritto in **due**
posti e i due devono coincidere:

**A — la scheda**, nella sezione 02:

```html
<div class="route-head">
  <span class="route-name display">Musica</span>          <!-- ← il tuo ambito -->
  <span class="route-tag mono">Hobby</span>               <!-- ← una glossa breve -->
</div>
```

**B — la riga del log**, nella sezione 03, stesso ordine, righe da 01 a 04:

```html
<div class="col-label">
  <span class="num mono">01</span>
  <span class="text">Musica</span>                        <!-- ← deve combaciare con A -->
</div>
```

Le quattro di serie sono `Musica` (Hobby), `Progetti` (Extra, più di un hobby),
`Studio` (Università), `Proattività` (Sport, cultura, lettura). Scegli quattro
ambiti davvero distinti fra loro: quattro sfumature di "lavoro" rendono il log
inutile, perché non saprai quale colonna segnare.

**Le righe 05 e 06 non sono rotte.** `Famiglia ✓` è la riga protetta e
`Not-to-do ✓` è la riga limite. Non hanno una scheda nella sezione 02, non hanno
traguardo, e mantengono le classi `log-row-protected` / `log-row-limit`. Puoi
rinominare `Famiglia` con qualunque sia il tuo non-negoziabile, ma lascialo un
vincolo: non dargli un traguardo.

### I riquadri della baseline

Sezione 01, quattro blocchi `.tile`. Cambia le descrizioni per farle aderire a
come ragioni davvero:

```html
<div class="tile tile-protected">
  <div class="tile-kicker mono">non negoziabile</div>
  <div class="tile-title display">Protetto</div>
  <div class="tile-note">Famiglia &amp; impegni fissi</div>   <!-- ← il tuo -->
  <div class="tile-fill"></div>
</div>
```

Lascia il primo riquadro verde (`tile-protected`) e il terzo rosso
(`tile-limit`). Sono i due poli che rendono realistico tutto il resto.

### La legenda

Sezione 03, `.legend`. Sei simboli più una casella vuota:

```html
<span class="legend-item"><span class="glyph">⚡</span>fatto con slancio</span>
```

Cambia le emoji, cambia le diciture. **Sei voci sono quante ce ne stanno**: la
settima schiaccia a zero la linea puntinata finale. Tieni l'ultimo blocco
`legend-custom`: la casella vuota è dove ti inventi un simbolo a mano, sulla
carta.

Non trasformarli in una scala di voti. `🧱 bloccato` e `✕ salto` sono due
*fatti* diversi, e quella differenza è tutto il senso del log.

### La testata

```html
<h1 class="display">La Settimana Possibile</h1>
<p class="masthead-sub">Si pianifica prima, si fa il consuntivo dopo. …</p>
```

Il `/ 52` accanto al campo settimana presuppone che tu numeri le settimane
sull'anno. Cambialo o toglilo come preferisci.

---

## 2. Ricolorarlo

Modifica il blocco `:root`. Gli undici token della palette raggiungono ogni
regola del file: non devi mai andare a caccia di esadecimali più in basso.

```css
:root {
  --paper:      #EEEAE0;   /* fondo del foglio */
  --paper-tile: #F5F2EA;   /* riquadri neutri */
  --dot:        #DCD6C6;   /* reticolo di puntini */
  --ink:        #26344A;   /* testo, bordi, filetti principali */
  --ink-soft:   #C9C2B2;   /* filetti secondari, separatori interni */
  --accent:     #B5533C;   /* numerazione, occhielli, evidenze */

  --protected:      #2F4A3C;   /* verde: non negoziabile */
  --protected-bg:   #DCE8DE;
  --protected-soft: #B7CCBB;

  --limit:      #9C4A4A;       /* rosso: ciò che eviti */
  --limit-bg:   #F3E4E0;
  --limit-soft: #DFC4BD;
}
```

**Conserva la semantica.** `--protected` deve leggersi come *sicuro / non
negoziabile* e `--limit` come *stop / evita*. Se li sostituisci con due colori a
caso non si rompe niente, ma il foglio smette di significare qualcosa a colpo
d'occhio.

Due alternative pronte:

<details>
<summary><strong>Prussia</strong> — più fredda, contrasto più alto</summary>

```css
--paper: #EDEAE3;  --paper-tile: #F6F4EE;  --dot: #D6D2C7;
--ink: #1F3A5F;    --ink-soft: #C2BFB4;    --accent: #C1663F;
--protected: #35594A;  --protected-bg: #D9E6DD;  --protected-soft: #B0C8B8;
--limit: #8F4545;      --limit-bg: #F2E1DD;      --limit-soft: #DCC0BA;
```
</details>

<details>
<summary><strong>Grafite</strong> — quasi monocromatica, la più economica da stampare</summary>

```css
--paper: #F2F1EE;  --paper-tile: #FAF9F7;  --dot: #DAD8D3;
--ink: #2B2B2B;    --ink-soft: #C6C4BF;    --accent: #6E6A63;
--protected: #3F4A3F;  --protected-bg: #E3E7E1;  --protected-soft: #C3CBC2;
--limit: #5A4444;      --limit-bg: #ECE4E2;      --limit-soft: #D2C3C0;
```
</details>

**Stampi a getto d'inchiostro?** Il fondo carta copre il 100% di un A3. Se lo
stampi ogni settimana, metti `--paper: #FFFFFF` e `--paper-tile: #FAFAF8`:
mantieni la struttura e le righe colorate e consumi una frazione dell'inchiostro.

---

## 3. Cambiare i font

Due modifiche coordinate: il `<link>` nell'`<head>` scarica i file, i token
`:root` ne dichiarano l'uso. Se ne cambi uno solo ripieghi in silenzio sui font
di sistema.

```css
--font-display: 'Space Grotesk', system-ui, sans-serif;   /* titoli */
--font-body:    Inter, system-ui, sans-serif;             /* testo corrente */
--font-mono:    'IBM Plex Mono', ui-monospace, monospace; /* etichette, numeri */
```

Il carico grosso lo porta `--font-mono`: tutte le micro-etichette maiuscole del
foglio usano quello. Scegli un monospaziato con un minuscolo vero e una
spaziatura comoda — le etichette sono a 6 pt e un carattere stretto in stampa
diventa poltiglia.

**Lo vuoi funzionante offline?** Togli il `<link>`, scarica i tre file `.woff2` e
incorporali come regole `@font-face` in base64. Il file passa da ~30 KB a
~350 KB e diventa davvero autonomo.

---

## 4. Togliere i crocini

I quattro crocini d'angolo servono a tagliare il foglio a misura. Per
nasconderli:

```html
<body class="no-fiducials">
```

Lo stesso vale per la linea `· strappa qui ·`: cancella il blocco `.tearline` se
non devi strappare niente.

---

## 5. Tradurlo

Il foglio è in italiano. Se lo traduci, **traducilo tutto**: un foglio mezzo
italiano sembra un bug. Le stringhe, in ordine di documento: la linea di
strappo, occhiello / titolo / sottotitolo della testata, `Nome` e `Settimana`,
le quattro etichette di sezione `01`–`04`, i riquadri della baseline, i campi
delle rotte (`Traguardo della settimana`, `Quando`, `Rituale d'inizio, 60 sec`),
titolo e nota del log, le abbreviazioni dei giorni `Lun…Dom` e `Tot`, la
legenda, i due titoli del consuntivo e la riga del punteggio (`Punteggio
settimana`, `cerchia un numero`, `umore della settimana`, `una cosa che
cambio`).

Aggiorna anche `<html lang="it">` e il `<title>`.

Occhio alle larghezze: le etichette monospaziate stanno in una colonna fissa da
34 mm e in campi a larghezza fissa. Tedesco e finlandese sbordano dove
l'italiano ci sta — controlla col [§7](#7-verificare-il-lavoro), non a intuito.

---

## 6. Cambiare formato carta

⚠️ **Due punti, e non si possono collegare.** La regola `@page` non legge le
custom property, quindi vanno modificati entrambi insieme:

```css
:root { --sheet-w: 420mm; --sheet-h: 297mm; }
@page { size: 420mm 297mm; margin: 0; }
```

**Questo da solo non ti dà una versione A4.** Il layout è tarato a mano in unità
assolute: le etichette da 6 pt restano 6 pt su un foglio più piccolo del 30%, le
altezze fisse in `mm` restano dove sono, e il contenuto sborda. Una vera
variante A4 significa ritarare la scala tipografica e ogni altezza fissa — un
lavoro serio, non una riga da cambiare.

Se ti serve su A4 oggi, lascia l'A3 e scegli **"adatta alla pagina"** nel dialogo
di stampa. Tutto rimpicciolisce del ~29%, le etichette da 6 pt finiscono intorno
a 4.2 pt: leggibile ma tirato. Stampane una e decidi con quella in mano.

---

## 7. Verificare il lavoro

Il foglio è specificato in millimetri, quindi "sembra a posto" non è una
verifica. Apri `index.html` in Chrome, apri i DevTools ed esegui lo snippet di
verifica da [`../AGENTS.md` §5](../AGENTS.md#5-verification--mandatory).

I due guasti che a schermo non si vedono:

- **sbordamento del contenuto** — un testo più lungo dell'originale spinge oltre
  il foglio e viene tagliato in silenzio da `overflow: hidden`;
- **una seconda pagina bianca** in stampa, per arrotondamento sub-millimetrico.

Quindi chiudi sempre con `Ctrl+P` e conferma che il dialogo dica **1 pagina**.

---

## 8. Darlo in mano a un agente AI

Fai partire l'agente da [`../AGENTS.md`](../AGENTS.md): contiene i vincoli rigidi
(file singolo, niente JS, niente build, esattamente 420 × 297 mm) e la procedura
di verifica. Senza, gli agenti provano puntualmente a "modernizzare" tutto
questo in una app React con una pipeline di build.

**Ricolorare:**

```
Leggi prima AGENTS.md. In index.html ricolora il foglio verso <descrivi il tono>
modificando solo i token della palette in :root — non scrivere esadecimali
dentro le regole. Mantieni --protected leggibile come "sicuro/non negoziabile"
e --limit come "stop/evita". Poi esegui la verifica di AGENTS.md §5 e riportami
i numeri.
```

**Cambiare i contenuti:**

```
Leggi prima AGENTS.md. In index.html sostituisci le quattro rotte con:
  01 <nome> (<glossa breve>)
  02 <nome> (<glossa breve>)
  03 <nome> (<glossa breve>)
  04 <nome> (<glossa breve>)
Ogni nome compare DUE volte — la scheda nella sezione 02 e la riga di log
corrispondente nella sezione 03. Aggiornale entrambe mantenendo l'ordine
allineato. Non toccare le righe 05 (protetta) e 06 (limite). Poi esegui la
verifica di AGENTS.md §5.
```

**Tradurre:**

```
Leggi prima AGENTS.md. Traduci in <lingua> ogni stringa visibile di index.html,
comprese la linea di strappo, le abbreviazioni dei giorni e la legenda — non
lasciare niente in italiano. Aggiorna <html lang> e <title>. Poi esegui la
verifica di AGENTS.md §5 e conferma che nessuna etichetta sbordi la sua colonna.
```

Qualunque sia il compito, pretendi che l'agente **ti riporti i numeri del §5**.
Un agente che dice "fatto, sembra a posto" non ha misurato niente, e questo è un
documento dove mezzo millimetro di errore ti costa un secondo foglio di A3.
