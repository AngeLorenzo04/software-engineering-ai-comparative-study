# Report di Analisi Statica del Software e Metriche di Design

Questo report fornisce una valutazione quantitativa e qualitativa del backend Express.js della web app NetScore26, calcolando le metriche di design del software per uno studio accademico comparativo. L'analisi esclude la cartella `node_modules`, i file di configurazione (`jest.config.js`, `package.json`, ecc.) e si concentra esclusivamente sul codice sorgente dell'applicazione.

---

## 1. Dimensione del Codice (LOC Metrics)

La tabella seguente mostra il dettaglio delle Linee di Codice (LOC) e delle Linee di Codice non di commento (NCLOC) per ogni file sorgente analizzato.

| File | LOC (Lines of Code) | NCLOC (Non-Comment LOC) |
| :--- | :---: | :---: |
| `index.js` | 12 | 6 |
| `src/app.js` | 47 | 33 |
| `src/controllers/authController.js` | 101 | 77 |
| `src/controllers/leagueController.js` | 266 | 228 |
| `src/controllers/matchController.js` | 68 | 58 |
| `src/controllers/predictionController.js` | 29 | 24 |
| `src/controllers/userController.js` | 192 | 164 |
| `src/controllers/webhookController.js` | 35 | 27 |
| `src/integrations/footballApiAdapter.js` | 69 | 57 |
| `src/jobs/matchSyncJob.js` | 230 | 194 |
| `src/jobs/runSync.js` | 14 | 11 |
| `src/middlewares/authMiddleware.js` | 23 | 18 |
| `src/middlewares/webhookAuth.js` | 42 | 23 |
| `src/routes/authRoutes.js` | 9 | 6 |
| `src/routes/leagueRoutes.js` | 22 | 10 |
| `src/routes/matchRoutes.js` | 10 | 6 |
| `src/routes/predictionRoutes.js` | 10 | 6 |
| `src/routes/userRoutes.js` | 16 | 8 |
| `src/routes/webhookRoutes.js` | 9 | 5 |
| `src/services/predictionService.js` | 89 | 75 |
| `src/services/scoringService.js` | 114 | 90 |
| `src/strategies/classicScoring.js` | 21 | 15 |
| `src/strategies/scoringStrategy.js` | 8 | 6 |
| `src/websockets/socketManager.js` | 40 | 32 |
| **Totale del Progetto** | **1377** | **1109** |

---

## 2. Complessità Ciclomatica (McCabe)

La complessità ciclomatica misura il numero di percorsi linearmente indipendenti attraverso il flusso di controllo. È stata calcolata a livello di singola funzione/endpoint tramite la formula   M = V(G) = d + 1, dove d è il numero di punti di decisione (`if`, `for`, `while`, `catch`, `case`, operatori logici `&&` e `||`, e operatore ternario `?`).

* **Complessità Ciclomatica Media del Progetto:** **7,13**

### Classifica delle Funzioni con Complessità Più Alta

| Posizione | File | Funzione / Endpoint | Valore McCabe |
| :---: | :--- | :--- | :---: |
| **1** | `src/jobs/matchSyncJob.js` | `syncMatches` | **19** |
| **2** | `src/integrations/footballApiAdapter.js` | `transformMatches` | **18** |
| **3** | `src/services/predictionService.js` | `createPrediction` | **16** |

### Mappatura Completa di Complessità per Funzione

| File | Funzione / Metodo | Valore McCabe | Punti di Decisione Rilevati |
| :--- | :--- | :---: | :--- |
| `src/jobs/matchSyncJob.js` | `syncMatches` | 19 | 6 `if`, 3 `catch`, 1 `for`, 5 `||`, 2 `?`, 1 `&&` |
| `src/integrations/footballApiAdapter.js` | `transformMatches` | 18 | 5 `case`, 2 `catch`, 5 `&&`, 4 `||`, 2 `?` |
| `src/services/predictionService.js` | `createPrediction` | 16 | 7 `if`, 5 `for`, 2 `&&`, 1 `||`, 1 `catch` |
| `src/controllers/webhookController.js` | `handleNostradamusWebhook` | 9 | 3 `if`, 1 `catch`, 1 `for`, 2 `||`, 1 `&&` |
| `src/controllers/userController.js` | `updateProfile` | 8 | 6 `if`, 1 `catch` |
| `src/services/scoringService.js` | `processMatchResult` | 8 | 3 `if`, 3 `for`, 1 `||` |
| `src/controllers/authController.js` | `register` | 7 | 3 `if`, 1 `catch`, 2 `||` |
| `src/controllers/leagueController.js` | `joinLeague` | 7 | 4 `if`, 1 `catch`, 1 `for` |
| `src/controllers/matchController.js` | `getMatches` | 7 | 3 `if`, 1 `catch`, 1 `||`, 1 `?` |
| `src/jobs/matchSyncJob.js` | `scheduleNextSync` | 7 | 3 `if`, 1 `catch`, 1 `for`, 1 `case` |
| `src/controllers/authController.js` | `login` | 6 | 3 `if`, 1 `catch`, 1 `||` |
| `src/controllers/leagueController.js` | `createLeague` | 6 | 2 `if`, 1 `catch`, 1 `while`, 1 `||` |
| `src/controllers/predictionController.js` | `createPrediction` | 6 | 1 `if`, 1 `catch`, 3 `||` |
| `src/controllers/userController.js` | `getProfile` | 6 | 2 `if`, 1 `catch`, 1 `for`, 1 `?` |
| `src/controllers/userController.js` | `getOtherUserProfile` | 6 | 2 `if`, 1 `catch`, 1 `for`, 1 `?` |
| `src/middlewares/webhookAuth.js` | `webhookAuth` | 5 | 3 `if`, 1 `for` |
| `src/controllers/leagueController.js` | `getUserLeagues` | 4 | 1 `catch

[ignoring loop detection]

---

## 3. Accoppiamento (Coupling & Information Flow)

La tabella seguente analizza i 5 moduli principali del sistema. L'accoppiamento locale considera solo le dipendenze interne al progetto sorgente.
La complessità strutturale di Henry-Kafura è calcolata tramite la formula: 
$$\text{Complessità Strutturale} = (\text{Fan-in} \times \text{Fan-out})^2$$

| Modulo / File | Fan-in (Dipendenti) | Fan-out (Dipendenze Locali) | Fan-out Esterno (Librerie) | Henry-Kafura Complexity |
| :--- | :---: | :---: | :---: | :---: |
| `src/controllers/leagueController.js` | 1 | 0 | 1 | **0** |
| `src/controllers/userController.js` | 1 | 0 | 2 | **0** |
| `src/jobs/matchSyncJob.js` | 2 | 2 | 2 | **16** |
| `src/services/scoringService.js` | 2 | 2 | 1 | **16** |
| `src/services/predictionService.js` | 1 | 0 | 1 | **0** |

### Analisi delle Tipologie di Accoppiamento
* **Common Coupling (Accoppiamento Comune)**: È presente un accoppiamento comune attraverso l'istanziamento globale e condiviso di `PrismaClient` (es. `const prisma = new PrismaClient()`) all'interno di molteplici moduli. Pur essendo un pattern standard in Prisma, l'interazione diretta con lo stato del database condiviso configura una dipendenza indiretta tra moduli che agiscono sulle medesime tabelle.
* **Content Coupling (Accoppiamento di Contenuto)**: Non si rileva Content Coupling diretto (ovvero un modulo che modifica direttamente il codice o lo stato interno privato di un altro modulo). Le interazioni avvengono tramite l'esportazione di funzioni pubbliche ed entry-point standard.

---

## 4. Coesione (Cohesion)

L'analisi della coesione valuta quanto le istruzioni all'interno di un modulo siano correlate tra loro.

### 1. `src/controllers/leagueController.js` (266 LOC)
* **Livello di Coesione**: **Coesione Logica**
* **Motivazione**: Il file raggruppa logicamente diverse operazioni relative alle leghe (creazione, unione, visualizzazione della classifica, cancellazione). Le funzioni non condividono un flusso di esecuzione sequenziale diretto tra di loro ma sono correlate logicamente dallo scopo comune dell'entità `League`.

### 2. `src/jobs/matchSyncJob.js` (230 LOC)
* **Livello di Coesione**: **Coesione Temporale**
* **Motivazione**: Il modulo contiene funzioni finalizzate all'esecuzione coordinata nel tempo di attività legate alla sincronizzazione (all'avvio del server, tramite cron quotidiano o allo scadere del timer relativo all'inizio del match).

### 3. `src/controllers/userController.js` (192 LOC)
* **Livello di Coesione**: **Coesione Logica**
* **Motivazione**: Accorpa metodi logicamente correlati alla gestione del profilo dell'utente (aggiornamento dei dettagli personali, recupero del proprio profilo o consultazione dei dati relativi ad altri utenti).

---

## 5. Duplicazione del Codice (Code Clones)

L'analisi ha rilevato una parziale duplicazione del codice all'interno dei controller e dei servizi.

* **Percentuale stimata di ridondanza (righe duplicate/totali)**: **~6.5%**

### Istanze Critiche Rilevate

1. **Cloni di Tipo-1 (Copia-Incolla Identico)**:
   * **`src/controllers/leagueController.js` e `src/controllers/userController.js`**:
     Si osserva la duplicazione esatta del blocco di query Prisma per recuperare le informazioni relative alla partecipazione alle leghe e il calcolo del ranking dei membri tramite ordinamento in memoria.
     ```javascript
     const userLeagues = await prisma.leagueMember.findMany({
       where: { userId },
       include: {
         league: {
           include: {
             leagueMembers: {
               select: {
                 userId: true,
                 totalPoints: true,
               },
             },
           },
         },
       },
     });
     ```

2. **Cloni di Tipo-2 (Variazioni Sintattiche o Variabili Rinominate)**:
   * **`src/controllers/userController.js` (Metodi `getProfile` e `getOtherUserProfile`)**:
     I due metodi condividono quasi la totalità del blocco logico di costruzione della risposta per le leghe, con variazioni unicamente nella proiezione dei campi utente ritornati (nel secondo metodo viene escluso l'indirizzo email e alcuni dettagli amministrativi della lega).
