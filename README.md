# RCU-W — Rename Comics Universal for Windows 3.1.0

[Italiano](#italiano) | [English](#english)

Rinomina fumetti con anteprima, regole personali e annullamento.
Rename comics with preview, custom rules and undo.

## Download

Download e supporto / Download and support — Buy Me a Coffee:

https://buymeacoffee.com/fconidi/e/485941

<img width="230" height="230" alt="rcu-qr-windows" src="https://github.com/user-attachments/assets/2ad91a05-c2e9-40b0-a1fb-fbfd90d82e8e" />

## Italiano


RCU rinomina fumetti PDF, CBR, CBZ ed EPUB. La versione 3.1.0 offre una
GUI ridimensionabile con anteprima, selezione dei file, regole guidate e
annullamento dell'ultima rinomina. GUI e console usano lo stesso motore.

### Avvio e utilizzo

Scarica la distribuzione Windows dal link Buy Me a Coffee qui sopra. Se è uno ZIP,
estrailo e apri **RCU-Win-3.1.0.exe** con doppio clic come utente normale.
Nel pacchetto l'eseguibile può essere chiamato **RCU-Win.exe**.
I moduli della GUI sono incorporati: non servono script esterni, Python o Go.
All'avvio i componenti vengono estratti in una cartella temporanea e rimossi
alla chiusura; preferenze e registri restano nel profilo utente.

Servono **Windows 10/11 x64**, Windows PowerShell 5.1 e Windows Forms,
già inclusi normalmente nel sistema, e il permesso di scrittura nella cartella
dei fumetti e nel profilo utente.

La GUI rileva la lingua dell'interfaccia di Windows tramite `Get-UICulture`:
IT, ES, DE, FR o EN. Le altre lingue usano EN. Sono riconosciute anche varianti
regionali, come `it-CH`, `es-MX`, `de-AT` e `fr-CA`. Il parametro `-Language`
accetta `auto`, `en`, `it`, `es`, `de`, `fr`; la scelta esplicita prevale
sul rilevamento automatico. Anche l'editor e i messaggi del motore mostrati
nella GUI seguono la lingua selezionata.
Riferimento: [Get-UICulture di Microsoft](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-uiculture).

1. Aprire **RCU-Win-3.1.0.exe** (oppure **RCU-Win.exe**, secondo il nome nel pacchetto).
   Si può anche trascinare una cartella sul launcher o nella finestra dell'app.
2. Scegliere la cartella. RCU propone la serie rilevata; se la cartella contiene
   serie diverse, lasciare vuota l'intestazione per conservare quella di ciascun file.
   Per nomi come `001-La morte bionda.cbr`, inserire la serie manualmente.
3. Aprire **Regole personali** per eliminare note o correggere parole.
4. Premere **Aggiorna anteprima** e confrontare nome attuale e nome proposto.
5. Selezionare i file desiderati e premere **Rinomina selezionati**.
6. Per ripristinare i nomi, usare **Annulla ultima rinomina** nella stessa cartella.

L'anteprima non cambia i file. Le righe già corrette o in conflitto sono escluse;
per i duplicati occorre scegliere una sola versione. Modificare un'opzione
richiede una nuova anteprima. Si può esportare la tabella in CSV.
L'elaborazione avviene in background; il numero dell'albo viene formattato con
almeno tre cifre. Non vengono visitate le sottocartelle.

### Come inserire i pattern

Per iniziare, scegliere un **Esempio pronto** e provarlo sul nome di un file.
Il campo **Testo da cercare** accetta testo normale senza virgolette:
parentesi, punti e `$` non hanno significati speciali. Lasciare disattivata
la modalità regex per le regole semplici.

| Obiettivo | Operazione | Testo da cercare | Sostituisci con | Dove applicare |
| --- | --- | --- | --- | --- |
| Togliere `scanpluto` e le note successive | Elimina testo | `scanpluto` | — | Da questo testo alla fine |
| Togliere uno scanner specifico | Elimina testo | `scan-pippo` | — | Da questo testo alla fine |
| Togliere una firma specifica | Elimina testo | `scan by giggi` | — | Da questo testo alla fine |
| Togliere una nota finale | Elimina testo | `-(sbc)` | — | Solo alla fine del nome |
| Togliere editore, data e note successive | Elimina testo | `(SBE` | — | Da questo testo alla fine |
| Correggere un errore | Sostituisci testo | `Il Regnio` | `Il Regno` | Ovunque nel nome |

**Ovunque** modifica tutte le occorrenze. **Solo alla fine** interviene se il testo
chiude il nome senza estensione. **Da questo testo alla fine** elimina o
sostituisce anche tutto ciò che segue quel testo: controllare che non faccia
parte del titolo. L'estensione del fumetto viene sempre conservata.

Il risultato si aggiorna mentre si scrive. Premere **Aggiungi regola**, poi
**Usa e salva regole**. Per scartare una bozza usare **Nuova**; **Annulla** chiude
l'editor senza applicarne le modifiche. Le regole si applicano dall'alto in basso;
si possono modificare, spostare o rimuovere. Gli esempi includono le note tra
parentesi o quadre del file selezionato: sono suggerimenti da valutare.
Il risultato nell'editor mostra le sole regole; l'anteprima principale comprende
anche numero, intestazione, spazi e pulizia automatica.

**Modalità avanzata (regex):** ad esempio `\((?:scan|c2c)[^)]*\)` elimina note
tra parentesi che iniziano con `scan` o `c2c`. Per questa regola scegliere
**Elimina testo**, **Ovunque** e attivare la modalità regex. Nelle sostituzioni
regex `$1` e `$2` indicano gruppi catturati. Le espressioni non valide sono
segnalate; ogni applicazione ha un timeout di 250 ms.

### Pulizia automatica e scanner

Con **Rimuovi metadati riconosciuti** attivo, vengono rimossi metadati riconosciuti
come editore con data, `Rocky V`, `Rocky VI`, `c2c`, `scan`, `scan2` e alcune firme.
La pulizia elimina il marcatore e il resto della coda del titolo.
Il marcatore viene riconosciuto dopo uno spazio, un trattino, un punto o una
parentesi: funziona anche con `Titolo-scan-pippo`.

| Nome iniziale | Risultato |
| --- | --- |
| `Zagor 733 - Il regno scan-pippo.pdf` | `Zagor 733 - Il regno.pdf` |
| `Zagor 733 - Il regno scan by giggi c2c.pdf` | `Zagor 733 - Il regno.pdf` |
| `Zagor 733 - Il regno scanpluto.pdf` | Rimane `scanpluto`; usare la regola specifica |
| `Zagor 733 - Uno scandalo.pdf` | `scandalo` viene conservato |

Le parentesi dei titoli, come `(prima parte)`, e gli anni contenuti nei titoli
sono conservati. **Iniziali maiuscole** è facoltativo: normalmente RCU mantiene
le maiuscole e minuscole del titolo. L'intestazione inserita manualmente prevale
sulla serie letta dal nome.

### Preferenze e annullamento

Regole e opzioni si salvano in `%LOCALAPPDATA%\RCUWin\settings.json`.
I registri delle rinomine si trovano in `%LOCALAPPDATA%\RCUWin\history`.
La rinomina controlla destinazioni esistenti e file cambiati dopo l'anteprima;
non sovrascrive file. L'annullamento verifica il contenuto con SHA-256 e rifiuta
file modificati o nomi originali occupati. Conservare i registri finché serve
poter annullare. I collegamenti sono esclusi.

Il formato delle regole salvate è JSON.

### Lingua da riga di comando

La lingua automatica segue Windows. Per sceglierla esplicitamente, apri
PowerShell nella cartella dell'EXE:

```powershell
.\RCU-Win-3.1.0.exe 'C:\Fumetti' -Language it
```

Valori disponibili: `auto`, `en`, `it`, `es`, `de`, `fr`.
Se l'eseguibile nel pacchetto è chiamato `RCU-Win.exe`, usa quel nome nel comando.

### Problemi comuni

| Problema | Cosa controllare |
| --- | --- |
| Il trascinamento non funziona | Avvia come utente normale oppure usa Sfoglia. |
| Nessun fumetto trovato | Controlla la cartella e i formati: le sottocartelle non vengono visitate. |
| Serie assente o errata | Compila l'intestazione e aggiorna l'anteprima. |
| Nomi proposti duplicati | Seleziona una sola versione per ciascun nome proposto. |
| Le regole non compaiono nell'anteprima | Premi Usa e salva regole, poi Aggiorna anteprima. |
| Una firma scanner rimane | Aggiungi una regola letterale specifica e controlla il risultato. |
| Annullamento rifiutato | Controlla che il file sia invariato, il nome originale libero e i registri presenti. |
| L'EXE non apre la finestra | Verifica Windows x64 e Windows PowerShell 5.1; conserva l'eventuale messaggio di errore. |

Per segnalare un problema indica versione dell'app, versione di Windows,
nome iniziale, nome proposto ed eventuale messaggio di errore.

## English

RCU-W renames **PDF, CBR, CBZ and EPUB** comics using the series, issue number
and title in each filename. Names follow `Series 001 - Title.ext`.
File contents and formats remain unchanged.

### Requirements and quick start

You need **64-bit (x64) Windows 10/11**, Windows PowerShell 5.1, Windows Forms
and write access to the comics folder and your user profile. Python and Go
are not required. The executable embeds the GUI modules, extracts them
temporarily and removes them when the app closes.

1. Download the Windows package from the Buy Me a Coffee link above. Extract it if supplied as a ZIP.
2. Double-click **RCU-Win-3.1.0.exe** as a regular user. The packaged executable may be named **RCU-Win.exe**.
3. Browse for a comics folder or drag it into the window.
4. Leave **Series / header** blank to retain each file's series, or enter a shared header. Enter a series for filenames beginning with an issue number.
5. Configure **Remove recognized metadata**, optional **Title case**, and **Custom rules**.
6. Click **Refresh preview** and compare current and proposed names. Preview does not change files.
7. Select the files, click **Rename selected** and confirm.
8. To restore names, select the same folder and click **Undo last rename**.

Subfolders are excluded. Refresh the preview after changing options or rules.
For duplicate proposed names, select only one version. Already correct or
conflicting entries are excluded. **Export preview** saves the table as CSV.
Issue numbers use at least three digits; original title capitalization is
kept unless Title case is enabled.

### Custom rules

Open **Custom rules**, choose a ready-made example or enter ordinary literal
text without quotes. Parentheses, dots and `$` need no escaping in literal mode.

| Goal | Action | Text to find | Replace with | Scope |
| --- | --- | --- | --- | --- |
| Remove a scanner and following notes | Remove text | `scanpluto` | — | From this text to the end |
| Remove a signature | Remove text | `scan by giggi` | — | From this text to the end |
| Remove a final note | Remove text | `-(sbc)` | — | Only at the end of the name |
| Remove publisher and following notes | Remove text | `(SBE` | — | From this text to the end |
| Correct a title | Replace text | `Il Regnio` | `Il Regno` | Anywhere in the name |

Check the live result, click **Add rule**, arrange the rules and click
**Use and save rules**. Rules run in list order. Refresh the main preview
before renaming. The file extension is preserved.

Anywhere changes all occurrences; Only at the end matches the filename end
without its extension; From this text to the end includes the following tail.
**New** discards a draft and **Cancel** closes the editor without applying changes.
The editor previews custom rules only; the main preview also applies series,
numbering and automatic cleanup. Advanced regex mode supports captured replacement
groups such as `$1` and enforces a 250 ms timeout per application.

### Automatic cleanup and undo

Recognized metadata cleanup removes supported markers and the following tail.
`scan-pippo` and `scan by giggi` are recognized; joined tags such as `scanpluto`
need a specific custom rule. Genuine title parentheses, title years and words
such as `scandalo` are retained.

Existing destinations are never overwritten. Files changed after preview
are rejected; links are excluded. Undo checks SHA-256 content fingerprints
and refuses modified files or occupied original names. Undo restores names,
not contents. Keep the history files while you need undo.

- Settings and rules: `%LOCALAPPDATA%\RCUWin\settings.json`
- Undo history: `%LOCALAPPDATA%\RCUWin\history`

### Interface language

Version 3.1.0 detects the Windows display language and supports **English,
Italian, Spanish, German and French**, including regional variants. Other
languages fall back to English. The main window, rule editor and displayed
engine messages follow the selected language.

To choose a language explicitly, open PowerShell beside the EXE:

```powershell
.\RCU-Win-3.1.0.exe 'C:\Comics' -Language en
```

Use the downloaded executable's actual filename. Accepted values:
`auto`, `en`, `it`, `es`, `de`, `fr`.

### Troubleshooting

| Problem | What to check |
| --- | --- |
| Drag and drop fails | Run as a regular user or browse for the folder. |
| No comics found | Check the folder and supported extensions; processing is not recursive. |
| Missing or incorrect series | Enter a header and refresh the preview. |
| Duplicate proposed names | Select only one version per proposed name. |
| Rule changes are missing | Use and save rules, then refresh the main preview. |
| Scanner tag remains | Add a specific literal rule and check the result. |
| Undo refused | Check unchanged content, free original names and retained history. |
| No window opens | Check Windows x64 and Windows PowerShell 5.1; retain any error message. |

When reporting a problem, include app/Windows versions, original/proposed names
and any error message.

## Video della versione precedente / Previous-version video

![RCU-Windows — versione precedente / previous version](https://github.com/user-attachments/assets/91466ea5-865f-4492-a52e-d4d1f590d32d)

## Autore / Author

Franco Conidi aka Edmond — SysLinuxOS.
System Integrator, Network Engineer, IT Consultant, Blogger, Linux Developer.

[francoconidi.it](https://francoconidi.it) · [syslinuxos.com](https://syslinuxos.com)
