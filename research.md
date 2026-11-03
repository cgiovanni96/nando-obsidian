# Sync e applicazione delle proposte AI — V1 core PARA + Habits

Ricerca primaria del **3 novembre 2026** per [Verificare sincronizzazione e confine di applicazione delle proposte AI](https://linear.app/giocosta/issue/NAS-118/verificare-sincronizzazione-e-confine-di-applicazione-delle-proposte). Letti ticket e discussione, risultata vuota, oltre a glossario e ADR correnti. Ambito: Android, Windows e possibile Linux/Omarchy; flussi locali offline; aggiornamenti alla riapertura; AI che propone e applica dopo conferma. Il rapporto accerta proprietà e lacune: non sceglie servizio, budget, numero di vault, formato dati o architettura AI. Nessuna installazione, configurazione o prova su dispositivi.

## Esiti principali

- Sync e backup sono funzioni distinte; la presenza della cronologia non verifica un ripristino dell'intero sistema.
- Il merge di file non garantisce integrità di Task, Occorrenze e Contributi: serve un disegno esplicito dei loro conflitti.
- CLI e MCP possono leggere **e scrivere**; non implementano automaticamente la conferma richiesta dall'ADR-0003. Autenticazione, versione del file e consenso umano risolvono problemi differenti.

## Obsidian Sync: prezzi e capacità documentate

Prezzi USD per utente mostrati dalla [pagina ufficiale Sync](https://obsidian.md/sync), consultata oggi; non sono un preventivo finale in euro né includono una stima delle imposte:

| Piano | Fatturazione annuale | Fatturazione mensile | Vault / spazio / singolo file / storico note |
| --- | --- | --- | --- |
| Standard | $4/mese, cioè $48/anno | $5/mese | 1 / 1 GB / 5 MB / 1 mese |
| Plus | $8/mese, cioè $96/anno | $10/mese | 10 / 10 GB / 200 MB / 12 mesi |

Plus è espandibile fino a 100 GB; il prezzo corrente dell'espansione non è stato verificato in un account e non va ricavato da annunci storici. I dispositivi sono illimitati. Spazio e cronologia consumano la quota **complessiva dell'account**, non una quota indipendente per vault; al limite Sync smette di sincronizzare finché non si libera spazio. [Plans and storage limits](https://help.obsidian.md/sync/plans).

### File, configurazione e plugin custom

La documentazione consente selezione per immagini/audio/video/PDF e l'opzione per gli altri tipi. La cartella di configurazione del vault, normalmente `.obsidian`, è sincronizzabile; per i community plugin occorre abilitare le opzioni relative a elenco attivo/installato. File/cartelle il cui nome comincia con punto sono esclusi, salvo la cartella di configurazione: una futura `.nando/` **non** va presunta sincronizzata. Opzioni Sync ed esclusioni sono per dispositivo; molti cambi a plugin/configurazioni richiedono riavvio. [Sync settings](https://help.obsidian.md/sync/settings).

**Inferenza da verificare:** un plugin privato distribuito nella configurazione può sfruttare la replica dei suoi file compilati, ma non è una certificazione del suo ciclo di aggiornamento. Su Android/Windows/Linux vanno provati prima installazione, abilitazione, aggiornamento, eventuali impostazioni specifiche del dispositivo e riavvio. I dati canonici del dominio non devono essere confusi con cache e preferenze solo perché il plugin li salva vicino alla configurazione.

### Offline e conflitti

Obsidian lavora sui file locali offline e replica al ritorno online. Sync supporta le piattaforme interessate e sincronizza quando Obsidian è in esecuzione; un telefono offline non ha ancora le modifiche dell'altro dispositivo. La riapertura non equivale quindi a una fotografia globale già aggiornata. È sconsigliato far operare contemporaneamente due motori di sync sullo stesso vault. [Sync FAQ](https://help.obsidian.md/sync/faq).

La [gestione ufficiale dei conflitti](https://help.obsidian.md/sync/troubleshoot) distingue:

| File | Regola documentata |
| --- | --- |
| Markdown | Merge testuale mediante diff-match-patch, che può generare duplicati o problemi di formattazione. |
| Altri formati, incluso Canvas | Prevale la modifica più recente. |
| JSON delle **impostazioni Obsidian** | Merge per chiavi, sovrapponendo chiavi locali alle remote. Non è una promessa di merge degli oggetti JSON arbitrari di Nando. |

Da Obsidian 1.9.7 è configurabile anche la creazione di copie conflittuali per revisione; la preferenza va impostata su ogni dispositivo. Una stessa nota creata automaticamente all'avvio sui due dispositivi ha inoltre un caso documentato in cui la copia remota prevale senza merge. Nessuna di queste regole riconosce da sola un Contributo duplicato o una rettifica intenzionale.

### Recupero e backup

[Version history](https://help.obsidian.md/sync/version-history) permette ripristino di note, allegati, file eliminati e configurazioni; per gli allegati la conservazione delle vecchie versioni è due settimane. Il ripristino è documentato anche su mobile. Un dettaglio della sidebar — vedere l'ultimo autore tramite hover — è marcato desktop-only: non certificare identità letterale dell'intero catalogo Sync senza chiarire la rilevanza di questa differenza per il requisito mobile.

[File recovery](https://help.obsidian.md/plugins/file-recovery) mantiene snapshot locali fuori dal vault: normalmente almeno cinque minuti fra copie e sette giorni di conservazione, configurabili. Ripristina solo `.md` e `.canvas`; le copie non passano agli altri dispositivi. Non copre quindi automaticamente dati JSON o binari di un plugin. La [guida backup](https://help.obsidian.md/backup) raccomanda una copia separata e unidirezionale: la replica delle cancellazioni e la cronologia del sync non sostituiscono un backup. Resta da decidere cosa salvare e come verificarne il recupero, senza imporre qui uno strumento.

## Alternativa concreta: Syncthing + Syncthing-Fork Android

L'app originale [syncthing/syncthing-android](https://github.com/syncthing/syncthing-android) è archiviata dal **3 dicembre 2024**: non presentarla come client ufficiale attuale. Un'alternativa verificabile è **Syncthing-Fork di researchxxl**: [release v2.1.7.0](https://github.com/researchxxl/syncthing-android/releases/tag/v2.1.7.0), pubblicata il 21 ottobre 2026, commit indicato `f881907`; [F-Droid](https://f-droid.org/en/packages/com.github.catfriend1.syncthingfork/) riporta la stessa versione, aggiunta il 25 ottobre, Android 6+. Sono prove di distribuzione recente, non un audit o una garanzia della manutenzione futura. [Syncthing ufficiale](https://syncthing.net/downloads/) distribuisce il core per Windows e Linux.

La [FAQ primaria Syncthing](https://docs.syncthing.net/users/faq.html) descrive replica peer-to-peer di creazioni, modifiche e cancellazioni quando i dispositivi sono online contemporaneamente. Un nodo aggiuntivo disponibile può facilitare la propagazione asincrona, ma è un'opzione operativa, non un servizio già necessario. Non emerge un abbonamento al prodotto; restano eventuali costi propri di storage, nodo e gestione.

Syncthing conserva versioni divergenti come file `.sync-conflict-…`; non fa merge semantico dei Markdown o del JSON. Le copie conflittuali diventano file ordinari e vengono replicate. Differenze di maiuscole nei nomi possono creare problemi fra Linux e filesystem non sensibili al caso. [Understanding synchronization](https://docs.syncthing.net/users/syncing.html).

Il [versioning](https://docs.syncthing.net/users/versioning.html) è disattivato di default, per cartella/dispositivo; archivia modifiche ricevute da altri dispositivi, non la precedente copia di ogni modifica locale. [Le esclusioni](https://docs.syncthing.net/users/ignoring.html) risiedono in `.stignore`, non replicato automaticamente. Quindi `.obsidian`, cache, layout e file temporanei richiedono una politica esplicita, diversa dal pannello Sync di Obsidian. Da verificare su Android accesso al percorso del vault, condizioni batteria/rete e tempestività alla riapertura; nessuna prova reale eseguita.

## Confine del vault e aggregazione

Un [vault è una cartella locale](https://help.obsidian.md/vault), con note e [configurazioni proprie](https://help.obsidian.md/data-storage); i link interni sono locali al vault. Un vault unico facilita viste comuni del core PARA e associazioni Habits. Vault distinti separano dati/configurazioni operative ma richiedono decidere come ritrovare e aggregare informazioni fra contesti: non c'è qui una federazione cross-vault dimostrata. La selezione del vault nella CLI indirizza un comando, non unifica gli archivi.

Queste sono conseguenze progettuali, **non** la scelta personale/lavoro né una garanzia di isolamento del sistema operativo. [Obsidian dichiara](https://obsidian.md/help/plugin-security) che non può limitare affidabilmente i singoli plugin a permessi granulari: un filtro di vista non è un controllo d'accesso. Un requisito di isolamento sistemico non è stato espresso dall'utente e non viene aggiunto dalla ricerca.

## CLI, MCP e conferma dell'utente

La [CLI ufficiale](https://help.obsidian.md/cli) richiede installer 1.12.7+ e app desktop in esecuzione; il primo comando può avviarla. Include lettura/ricerca, creazione/modifica/eliminazione, gestione task, esecuzione di comandi e `eval`. Può indirizzare un vault per nome/ID e un file per percorso. È uno strumento operativo ampio, non un motore di proposte con approvazione incorporata. [Headless](https://help.obsidian.md/headless), oggi open beta, è invece un client indipendente per servizi Sync/Publish; la documentazione non lo presenta come runtime dei plugin.

Un esempio community attuale è [Local REST API](https://github.com/coddingtonbear/obsidian-local-rest-api), che documenta anche un server MCP integrato con autenticazione a token. Espone letture, scritture, patch, cancellazioni e comandi. Il README corrente rifiuta accesso alla configurazione per default e offre scritture condizionate: versione letta, `ifMatch`, errore se il file è cambiato; il REST usa ETag/If-Match. Queste proprietà dipendono dalla versione del plugin e vanno confermate nella release scelta: leggere `main` non certifica un'installazione. Nessun gate universale di conferma umana è documentato da questi strumenti.

Tre livelli vanno tenuti distinti:

| Livello | Cosa stabilisce | Cosa non dimostra |
| --- | --- | --- |
| Comportamento dell'agente | Preparare una modifica, mostrarla, aspettare il consenso, poi applicarla. | Il solo prompt non impedisce tutte le altre scritture disponibili. |
| Accesso effettivo | Quali file, tool e comandi sono accessibili nel client/server scelto. | Un token valido non significa che l'utente abbia approvato quel cambiamento. |
| Protocollo di applicazione | Collegare conferma, contenuto proposto, precondizioni e risultato. | Una versione del file non dimostra da sola consenso o atomicità fra più file/dispositivi. |

**Pattern possibili, ancora da scegliere:** revisione nel client prima della singola scrittura; proposta salvata come bozza e applicata dall'utente tramite un comando; oppure un applicatore dedicato che espone azioni mirate e verifica conferma/precondizioni. Costi e grado di vincolo differiscono. Non occorre assumere un ambiente isolato; occorre dichiarare quale comportamento viene controllato, quali accessi rimangono disponibili e come lo si proverà. Una proposta divenuta obsoleta dopo una modifica Android deve essere rivalutata, non applicata sul solo presupposto che il PC fosse aggiornato quando l'ha generata.

## Implicazioni per Task e Contributi: inferenze da verificare

La [Vault API](https://docs.obsidian.md/Plugins/Vault) offre `process()` per evitare modifiche concorrenti fra lettura e aggiornamento del singolo file locale. Non è una transazione distribuita.

- Identità di Task, Serie ricorrente e Occorrenza devono sopravvivere a spostamenti, merge e nuove copie; titolo, percorso e numero di riga non dimostrano da soli l'identità logica.
- Un completamento e la registrazione del relativo Contributo possono arrivare separatamente. Il caso richiede recupero dopo interruzione e deduplicazione; righe testuali duplicate non devono contare due azioni.
- Un JSON/DB centrale rende più frequente la contesa; file distinti per registrazione possono ridurla ma introducono riconciliazione e riconoscimento delle copie conflittuali. Sono alternative, non formati adottati.
- Una rettifica, una pausa e una modifica della frequenza fatte offline su dispositivi diversi richiedono precedenza esplicita e ricalcolo deterministico. La replica dell'ultima scrittura non decide quale intenzione preservare.

Prima della scelta finale servono prove mirate: stessa occorrenza completata offline due volte; contributi distinti offline; riapertura del task contro rettifica; rename durante una proposta AI; arresto dopo la prima scrittura; aggiornamento del plugin con impostazioni locali; ripristino coordinato di dati e configurazione. La ricerca non promette transazioni cross-device e lascia aperte le decisioni che questi casi devono guidare.
