# Proposta di progetto

> Una pagina, da far approvare al docente **prima** di scrivere codice. Una volta
> approvata, resta nel repository come traccia del punto di partenza (e si confronta
> con quello che hai effettivamente realizzato).

## Titolo provvisorio

Trasheet: Comegna Edition

## Gruppo

- Flavio Romanu
- Samuele Comegna

## Cosa fa (5-6 righe)

Project Cleaner è un'applicazione Python con interfaccia grafica semplice pensata per aiutare l'utente a gestire i file inutili che si accumulano durante lo sviluppo di un progetto. Il programma permette di selezionare una cartella, analizzarla e individuare file temporanei, cache, log, cartelle di build, file duplicati e altri elementi che non sono più necessari. Dopo la scansione mostra un report con il percorso dei file trovati, la dimensione e il motivo per cui sono stati segnalati. Prima di modificare qualsiasi file, l'utente può controllare il risultato e scegliere se simulare soltanto l'operazione, spostare i file in quarantena o procedere con la pulizia.

## A chi serve / quale problema reale risolve

Il programma serve a studenti, sviluppatori e persone che lavorano spesso su progetti software o scolastici. Durante lo sviluppo si accumulano facilmente file generati automaticamente, cartelle di cache, file di log, copie temporanee e duplicati. Questi elementi possono rendere il progetto più disordinato, occupare spazio inutile e creare confusione quando bisogna consegnare il lavoro o condividerlo con altri. Project Cleaner risolve questo problema offrendo un metodo controllato per individuare e gestire i file non necessari, riducendo il rischio di cancellazioni accidentali grazie al report, alla modalità dry-run e alla possibilità di spostare i file in quarantena.

## Competenze del corso messe in gioco

Nel progetto verranno utilizzate diverse competenze affrontate durante il corso:
OOP, per organizzare il programma in classi con responsabilità separate;
ereditarietà e polimorfismo, per rappresentare regole di pulizia diverse ma utilizzabili in modo uniforme;
classi astratte, per definire un contratto comune per tutte le regole di pulizia;
filesystem con pathlib e shutil, per esplorare cartelle, leggere metadati, spostare ed eliminare file;
file I/O e JSON, per salvare configurazioni e report strutturati;
CSV, per esportare i risultati della scansione in un formato leggibile anche da un foglio di calcolo;
regex, per riconoscere file e cartelle tramite pattern configurabili;
hashing con hashlib, per individuare file duplicati confrontando il contenuto;
SQLite con sqlite3, per salvare lo storico delle scansioni e delle operazioni effettuate;
query parametrizzate, per inserire e leggere dati dal database in modo ordinato e sicuro;
logging, per registrare scansioni, errori, file spostati o eliminati;
gestione delle eccezioni, per trattare errori come permessi mancanti, file non accessibili o percorsi non validi;
Tkinter, per realizzare un'interfaccia grafica semplice e usabile;
pytest, per testare la logica delle regole, del motore di scansione e del database.


## Gerarchia di ereditarietà prevista

La gerarchia principale sarà basata sulle regole di pulizia.
Classe base: CleanupRule — rappresenta una regola generica in grado di valutare un file o una cartella. Contiene il comportamento comune a tutte le regole, come nome, descrizione, livello di rischio e interfaccia del metodo di controllo.
Sottoclassi: TempFileRule, CacheRule, LogFileRule, BuildArtifactRule, DuplicateFileRule.
Metodo polimorfico: matches(file_info) — è il metodo che ogni sottoclasse ridefinisce per stabilire se un file deve essere segnalato. Il motore di scansione chiamerà matches() su una lista di regole senza conoscere la sottoclasse concreta.
Perché ereditarietà e non composizione/funzioni: ogni regola è realmente un tipo specifico di CleanupRule, quindi la relazione è di tipo is-a. Usare l'ereditarietà permette di aggiungere nuove regole creando nuove sottoclassi, evitando una singola funzione piena di if o match difficili da estendere.

## Piano di massima (fasi)

1) Struttura iniziale e motore di scansione
Creazione del repository con cartelle src, docs e tests. Implementazione della scansione ricorsiva di una cartella usando pathlib e raccolta delle informazioni principali sui file, come percorso, dimensione, estensione e data di modifica.
2) Regole di pulizia e report
Implementazione della classe base CleanupRule e delle prime sottoclassi, come TempFileRule, CacheRule e LogFileRule. Creazione di un report con percorso, dimensione e motivo della segnalazione. In questa fase il programma dovrà funzionare in modalità sicura, quindi senza modificare o cancellare file.
3) Interfaccia grafica e modalità sicura
Realizzazione della finestra principale con Tkinter: scelta della cartella, avvio scansione, visualizzazione dei risultati e opzioni dry-run e quarantena. Implementazione dello spostamento sicuro dei file tramite shutil, evitando cancellazioni dirette non confermate.
4) Duplicati, esportazione e logging
Aggiunta della regola per i duplicati tramite hashing con hashlib. Implementazione dell'esportazione del report in JSON e CSV. Aggiunta del logging per registrare scansioni, errori, file segnalati, file spostati in quarantena ed eventuali operazioni di pulizia.
5) Estensione avanzata con SQLite
Implementazione di un database locale SQLite per salvare lo storico delle scansioni. Il database potrà contenere informazioni come data della scansione, cartella analizzata, numero di file segnalati, spazio recuperabile, file spostati in quarantena e operazioni effettuate. Le query saranno gestite con parametri, evitando costruzioni insicure tramite stringhe formattate.
6) Test e documentazione
Scrittura dei test con pytest, in particolare sulle regole di pulizia, sul comportamento polimorfico e sulle funzioni principali collegate al database. Completamento di README, manuale utente, manuale tecnico, scelte implementative e devlog.


**A metà percorso conto di avere pronto:** 

Una versione funzionante del motore di scansione, capace di analizzare una cartella, applicare alcune regole di pulizia e produrre un report senza modificare i file.

## Estensione avanzata che vorrei tentare

Vorrei tentare l'implementazione di uno storico delle scansioni tramite SQLite. Il programma salverà in un database locale informazioni come data della scansione, cartella analizzata, numero di file segnalati, spazio recuperabile, file spostati in quarantena e operazioni effettuate. Questa estensione permetterà di consultare le scansioni precedenti, visualizzare statistiche sullo spazio recuperato nel tempo e rendere la funzione di quarantena più controllabile.
La scelta di SQLite è adatta perché non richiede un server esterno, salva i dati in un singolo file locale e permette di usare query parametrizzate per gestire le informazioni in modo ordinato e sicuro.
Come ulteriori estensioni possibili, vorrei valutare anche il ripristino dei file dalla quarantena, la configurazione personalizzata delle regole tramite file JSON e l'esclusione automatica di cartelle importanti come .git, .venv o venv.
