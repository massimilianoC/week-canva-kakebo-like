# La Settimana Possibile

**Un planner settimanale che sta su un foglio solo. Si compila a penna.**

🇬🇧 [Read this in English](README.md) · 🤖 [Istruzioni per agenti](AGENTS.md) · 🎨 [Guida alla personalizzazione](docs/PERSONALIZZA.md)

[![Licenza: AGPL v3](https://img.shields.io/badge/Licenza-AGPL_v3-blue.svg)](LICENSE)

![Il foglio](docs/preview.png)

---

## Cos'è

Un foglio A3 orizzontale (420 × 297 mm), una settimana, una penna. Si pianifica
sopra all'inizio della settimana e si chiude il cerchio sullo stesso foglio alla
fine — niente app, niente serie da non spezzare, niente notifiche.

È un solo `index.html` autonomo. Nessuna build, nessuna dipendenza, nessun
JavaScript. Lo apri nel browser, `Ctrl+P`, stampi.

Il foglio arriva **in italiano**, con i quattro ambiti di vita dell'autore già
scritti dentro. È una scelta voluta: un template pieno di segnaposto tipo
`Ambito 01` non ti insegna niente su come va usato. Sostituirli con i tuoi è la
prima cosa da fare — vedi [docs/PERSONALIZZA.md](docs/PERSONALIZZA.md).

## Il metodo

Il foglio ruota attorno a un'idea sola: **un piano che ignora i tuoi vincoli
veri non è un piano, è un desiderio.** Quindi i vincoli vengono per primi, e il
consuntivo si fa sulle prove invece che sui ricordi.

Cinque blocchi, nell'ordine in cui si usano:

| # | Blocco | Quando | A cosa serve |
| --- | -------- | -------- | -------------- |
| **01** | **Baseline** | Lunedì | Quattro riquadri: cosa è *protetto* (famiglia, impegni fissi), dove stanno davvero le tue *finestre* libere, il *not-to-do* della settimana e il *focus*. |
| **02** | **Obiettivi per ambito** | Lunedì | Quattro "rotte" — i tuoi ambiti. Ognuna ha un traguardo settimanale, un *quando* e un rituale d'inizio da 60 secondi. |
| **03** | **Log** | Ogni sera | 7 giorni × 6 righe. Segni cosa è successo davvero con dei simboli, non con dei voti. Ci vogliono venti secondi. |
| **04** | **Consuntivo** | Domenica | Cosa è andato bene, cosa è andato storto — letti dal log, non dall'umore. |
| **05** | **Punteggio** | Domenica | Un numero da 0 a 10, un'emoji per la settimana e una cosa che cambi. |

Tre dettagli che reggono quasi tutto il peso:

- **Il log registra fatti, non giudizi.** ⚡ *fatto con slancio*, 🌱 *piccolo
  passo*, 🎯 *traguardo centrato*, 🌀 *rimandato*, 🧱 *bloccato*, ✕ *salto*.
  "Bloccato" e "salto" sono due fatti diversi, e la differenza è proprio la
  parte utile. In fondo alla legenda c'è una casella vuota per inventarti un
  simbolo tuo.
- **La riga protetta si spunta, non si conta.** La famiglia è la riga 05, su
  fondo verde, senza traguardo. È un vincolo, non un indicatore: nel momento in
  cui le dai un punteggio hai iniziato a ottimizzare la cosa sbagliata.
- **Anche il not-to-do si spunta.** Riga 06, rossa. Una settimana in cui hai
  evitato quello che volevi evitare è una buona settimana, anche se i traguardi
  sono slittati.

## Stamparlo

Apri `index.html` nel browser e premi `Ctrl+P`. In basso a destra
nell'anteprima a schermo c'è un piccolo pulsante **"Scarica PDF"**: punta a una
copia pre-renderizzata e vuota dentro `examples/`, per chi lo vuole senza
generarselo da sé. Nessun JavaScript coinvolto: è un link di download statico
che sparisce da solo in stampa (`@media print`), quindi non compare mai sul
foglio stesso.

| Voce | Valore |
| --- | --- |
| Destinazione | *Salva come PDF*, oppure la tua stampante A3 |
| Formato carta | **A3** |
| Orientamento | **Orizzontale** |
| Margini | **Nessuno** |
| Scala | **Predefinita / 100%** — *non* "Adatta all'area stampabile" |
| Grafica di sfondo | **✅ attiva** |

**Di default, la stampa risparmia già inchiostro.** Il fondo carta copre il
100% di un A3: tinto, saturerebbe un foglio intero ogni settimana. Per questo
`index.html` porta i fondi neutri a bianco specificamente in `@media print`,
lasciandoli color carta a schermo: su carta resta il reticolo di puntini, la
struttura a solo inchiostro, e le due righe pensate per saltare all'occhio —
protetto (verde) e not-to-do (rosso), che insieme coprono circa un decimo del
foglio.

Vuoi comunque il fondo tinto su carta? Aggiungi `class="print-tinted"` a
`<body>` prima di stampare.

**"Grafica di sfondo" deve restare comunque attiva.** Se la spegni nel dialogo
di stampa perdi il reticolo di puntini e le due righe colorate insieme al fondo
carta: esce bianco con quattro righe sopra. Il CSS dichiara già
`print-color-adjust: exact`, ma l'ultima parola resta alla casella del browser.

Al primo caricamento serve la rete, per i tre font di Google. Se lo apri offline
la tipografia ripiega sui font di sistema e la spaziatura si sfalsa.

**Sulla stampa fisica:** il foglio è disegnato al vivo, il fondo carta arriva al
bordo dei 420 × 297 mm. Nessuna stampante A3 da scrivania ci riesce: tengono
tutte un margine meccanico di 3-5 mm. Stampa al 100% e accetta il bordo bianco —
i crocini agli angoli servono esattamente a tagliare il foglio a misura. Se
scegli "adatta alla pagina" invece rimpicciolisci tutto del 4% circa, etichette
da 6 pt comprese.

## Personalizzarlo

Tutto sta in un file solo, in due punti:

- il **blocco `:root`** in cima a `index.html` — colori, font, formato carta;
- il **corpo HTML** — le quattro rotte, i simboli della legenda, le diciture.

Istruzioni complete, scritte sia per umani sia per agenti di codice:
**[docs/PERSONALIZZA.md](docs/PERSONALIZZA.md)** · 🇬🇧 **[docs/CUSTOMIZE.md](docs/CUSTOMIZE.md)**

Se punti un agente AI su questa repository, fallo partire da
**[AGENTS.md](AGENTS.md)**: contiene i vincoli rigidi e la procedura di verifica
che tiene il foglio esattamente in A3.

## Struttura della repository

```text
.
├── index.html          ← il foglio. L'unico file che conta. Autonomo.
├── AGENTS.md           ← regole e verifiche per gli agenti AI
├── README.md           ← inglese
├── README.it.md        ← questo file
├── LICENSE             ← AGPL-3.0
├── docs/
│   ├── CUSTOMIZE.md    ← come farlo tuo (inglese)
│   ├── PERSONALIZZA.md ← come farlo tuo (italiano)
│   └── preview.png     ← il foglio renderizzato
└── examples/
    └── la-settimana-possibile.pdf   ← vuoto, pre-renderizzato — lo stesso
                                        file che serve il pulsante "Scarica PDF"
```

## Ispirazione

Il metodo prende in prestito da due tradizioni di pianificazione su pagina
singola, applicandole a un dominio diverso — una settimana invece di un bilancio
domestico o di un business:

- **[Kakebo](https://it.wikipedia.org/wiki/Kakeibo)** (家計簿) — il metodo
  giapponese di budgeting domestico, su carta, compilato a mano: ci si impegna
  su un piano all'inizio del periodo e lo si riconcilia con quanto successo
  davvero alla fine, sulla stessa pagina. Il ritmo pianifica-poi-consuntiva delle
  sezioni 01–02 contro 03–05 di questo foglio è un discendente diretto di quella
  disciplina.
- **[Lean Canvas](https://leanstack.com/lean-canvas)** (Ash Maurya) — un piano
  di business su una pagina sola, organizzato come griglia di riquadri
  numerati, a sua volta adattato dal
  **[Business Model Canvas](https://www.strategyzer.com/library/the-business-model-canvas)**
  (Alexander Osterwalder). L'idea che questo foglio conserva: costringere un
  piano dentro un insieme fisso di riquadri piccoli e numerati su una pagina
  sola, così non c'è spazio per essere vaghi e non c'è una pagina 2 in cui
  nascondersi.

Da dove viene il design *tecnicamente*, a differenza che concettualmente: il
foglio è nato come prototipo `doc-page` in
[Claude Design](https://claude.ai/design), poi è stato reimplementato a mano
come documento di stampa autonomo. Il runtime dello strumento (`doc-page.js`,
`support.js`, ~110 KB) è stato eliminato e il suo comportamento di page-box
ridotto a un normale `@page { size: 420mm 297mm; margin: 0 }`. Del design system
"modernist" su cui era nominalmente costruito è rimasto solo il reset CSS: il
prototipo ne sovrascriveva palette e tipografia da cima a fondo, quindi il resto
è stato lasciato indietro.

## Licenza

Rilasciato sotto **GNU Affero General Public License v3.0** — vedi [LICENSE](LICENSE).

In sintesi: usalo, stampalo, vendi le stampe, modificalo, costruicci sopra —
liberamente. Le condizioni sono che tu **citi l'autore** e che le **tue modifiche
restino aperte sotto la stessa licenza**, anche quando fai girare una versione
modificata come servizio di rete.

**Ti serve ad altre condizioni?** Se l'AGPL non va bene per il tuo caso — uso
proprietario, ridistribuzione chiusa, licenza commerciale — scrivimi e ci
mettiamo d'accordo: **[LinkedIn — Massimiliano Camillucci](https://www.linkedin.com/in/massimilianocamillucci/)**

Copyright © 2026 Massimiliano Camillucci
