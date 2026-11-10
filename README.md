[README.md](https://github.com/user-attachments/files/34641470/README.md)
# OnlyT per macOS

Versione nativa per Mac (Swift, AppKit + SwiftUI) di **OnlyT 2.5**, il timer per le adunanze di
Antony Corbett (originale per Windows: WPF/.NET). Stessa logica, stesse finestre, stesse API remote e
stesse pagine dell'orologio web; interfaccia in italiano (e nelle altre 37 lingue dell'originale).

---

## Creare l'app

Serve un Mac con **macOS 13 (Ventura) o successivo** e gli *Xcode Command Line Tools*
(`xcode-select --install`) oppure Xcode completo.

**Metodo 1 – doppio clic**
Fai doppio clic su `Crea OnlyT.command`. La prima volta macOS potrebbe bloccarlo: in quel caso tasto
destro → *Apri* → *Apri*.

**Metodo 2 – Terminale**

```bash
cd OnlyT-Mac
./Scripts/build_app.sh             # app per questo Mac
./Scripts/build_app.sh universal   # Intel + Apple Silicon (richiede Xcode completo)
```

Al termine trovi `build/OnlyT.app` (e `build/OnlyT-macOS.zip`). Trascinala in *Applicazioni*.

> Se copi l'app su un altro Mac e macOS dice che "non può essere aperta": tasto destro → *Apri*,
> oppure `xattr -dr com.apple.quarantine /Applications/OnlyT.app`.

**Sviluppo in Xcode**: `open Package.swift`, scegli lo schema *OnlyT* e premi ▶︎.
**Test** della logica (orari adattivi, parsing orari, feed, ecc.): `swift test`.

### Primo avvio
- Se attivi l'orologio web o le API, macOS chiede di **consentire le connessioni in entrata**: scegli *Consenti*.
- Usando la modalità "Basata su file" o i rapporti PDF, macOS chiede l'accesso alla cartella **Documenti**.

---

## Cosa fa (come l'originale)

**Finestra operatore**
- Elenco delle parti con durata e scarto finale (verde in anticipo / rosso in ritardo).
- Timer grande verde → giallo (fase di chiusura, 30 s o `closing` del file) → rosso (tempo scaduto).
- Durate *originale → modificata → adattata*.
- `+` / `−` = 1 minuto · con **⇧ Maiusc** = 5 minuti · con **⌥ Opzione** = 15 secondi
  (su Windows era Ctrl-clic, che su Mac è il clic destro). Doppio clic sul timer o ✎ per scrivere la durata.
- Rotella del mouse / trackpad sul timer per regolarlo (opzione).
- Avvio sincronizzato al secondo, pausa (opzione), conteggio avanti/indietro (opzione).
- Campanella: automatica allo scadere; cliccabile per attivarla/disattivarla o per suonarla a tempo scaduto.
- Interruttore "Visita sorvegliante", pulsante **CHIUDI CONTATEMPO**.
- **Compatta**: il pulsante giallo "riduci" della finestra la rimpicciolisce invece di mandarla nel Dock
  (opzione "Compatta con riduci a icona"), come lo "shrink" dell'originale.

**Display del timer** (secondo monitor a schermo intero, oppure in finestra)
- Orologio analogico identico all'originale (quadrante, lancette, animazione d'avvio) con il **settore
  colorato** della durata della parte e l'orario digitale.
- Tra una parte e l'altra orologio a schermo intero; durante una parte schermo diviso orologio + timer,
  con le stesse dissolvenze.
- "Mantieni tempo studente": il tempo finale resta visibile (con barra di scorrimento opzionale).
- Cornici, sfondi sfumati, orologio piatto, ora sotto il timer, formati 12/24 h, puntatore nascosto.

**Contatempo pre-adunanza**: parte da solo negli orari indicati ("gio 19:15", "dom 10:00", "19:15"…),
durata 1–60 min, quadrante e/o numeri, 9 posizioni, zoom, sfondo trasparente.

**Modalità**
- *Automatica*: programma infrasettimanale/fine settimana con le durate scaricate dal feed online di
  OnlyT (in cache per 20 giorni), visita del sorvegliante, **tempi adattivi** (ritardo / ritardo e anticipo),
  promemoria "Avresti dovuto avviare il timer?", notifiche di ritardo/anticipo, **rapporti PDF** dei tempi
  (con grafici storici degli ultimi 6 mesi).
- *Manuale*: un solo timer libero.
- *Basata su file*: programmi XML nella cartella *Documenti → OnlyT → TalkSchedules* (stesso formato
  di Windows; dalle Impostazioni puoi creare un esempio o aprire la cartella).

**Modifica del programma** *(novità, non presente nella versione Windows)*
Pulsante ☰ nella finestra operatore, «Modifica programma…» in fondo all'elenco delle parti, oppure
menu *Timer* (⌥⌘E). Puoi:
- **aggiungere**, **duplicare** ed **eliminare** parti;
- **spostarle** trascinandole o con le frecce ↑ ↓;
- **modificare** nome, sezione, durata, campanella, conteggio avanti/indietro, fase di chiusura (giallo),
  "mantieni tempo finale", parte di studente e tempo adattivo.

Le modifiche si applicano con **Applica** (a timer fermo) e restano valide per quel programma: in modalità
automatica per l'adunanza della settimana corrente, nelle altre modalità finché non premi
**Ripristina originale**. Le parti esistenti mantengono il loro identificativo, quindi tempi adattivi,
promemoria, rapporti e API remote continuano a funzionare. Con **Salva come file…** il programma diventa un
file XML riutilizzabile (anche con OnlyT su Windows); il pulsante **USA** passa subito a quel file.

**Rete**
- Orologio web su `http://<IP-del-Mac>:8096/index` e pagina dei timer `/timers`, con codice QR.
- API REST v1–v4 compatibili (`/api/v4/timers`, `/bell`, `/datetime`, `/system`, durata,
  codice di accesso, limitazione richieste, CORS): le app di controllo remoto esistenti funzionano.

### Scorciatoie (menu *Timer*)
⌘↩ avvia/ferma · ⌘[ / ⌘] parte precedente/successiva · ⌘= / ⌘− ±1 minuto · ⌘E modifica durata ·
⌘B campanella · ⌘P pausa (se attiva) · ⌥⌘E modifica programma · ⌘K compatta · ⌘, impostazioni.

### Opzioni di avvio (come su Windows)
```bash
open -a OnlyT --args --datetime "2026-11-12 19:10"   # simula data/ora
open -a OnlyT --args --covisit                       # visita del sorvegliante
open -a OnlyT --args --feed https://…/feed.json      # feed alternativo
```

### Dove salva i dati
| Cosa | Percorso |
|---|---|
| Impostazioni | `~/Library/Application Support/OnlyT/options.json` |
| Programmi modificati | `~/Library/Application Support/OnlyT/CustomSchedules.json` |
| Cache del feed / dati dei tempi | `~/Library/Application Support/OnlyT/` |
| Programmi XML | `~/Documents/OnlyT/TalkSchedules/` |
| Rapporti PDF | `~/Documents/OnlyT/TimingReports/` |

---

## Differenze rispetto alla versione Windows
- **Non incluse**: uscita video **NDI** (libreria solo Windows), l'utility per le porte del firewall
  (su Mac il permesso lo chiede il sistema), il controllo automatico degli aggiornamenti e i log Serilog/Sentry.
- Il database LiteDB dei tempi è sostituito da un file JSON; il PDF è generato con Core Graphics.
- La lingua si cambia dalle Impostazioni (o segue quella del sistema).

## Struttura del progetto
```
Package.swift
Sources/OnlyTCore/   logica indipendente dall'interfaccia (opzioni, programmi, feed,
                     tempi adattivi, motore timer, promemoria, dati dei tempi, HTTP + API)
Sources/OnlyT/       app macOS (finestre, viste SwiftUI, orologio, contatempo, server web, PDF)
Resources/           traduzioni, pagine dell'orologio web, campanella, programma d'esempio
Packaging/           Info.plist e icona
Scripts/build_app.sh crea OnlyT.app
Tests/               test della logica (porting dei test originali)
```

## Licenza
OnlyT © 2018–2026 Antony Corbett e collaboratori, licenza MIT (vedi `LICENSE-OnlyT-original.txt`).
Questa versione per macOS è un porting basato sul codice sorgente della 2.5.0.10.
