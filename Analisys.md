# REPORT DI ANALISI STATICA E ARCHITETTURA DEL SOFTWARE
**Progetto:** PredictGame Backend \
**Stack:** Express.js + Prisma TypeScript Backend\
**Ambito di studio:** Studio Accademico Comparativo sulle Metriche di Qualità del Software

---

## 1. Dimensione del Codice (LOC Metrics)

L'analisi dimensionale della codebase ha esaminato tutti i file sorgente `.ts` presenti all'interno della directory `src/`. Sono stati esclusi i file di configurazione (`tsconfig.json`, `package.json`), i moduli esterni (`node_modules`), gli output di compilazione (`dist`, `build`) e la definizione dello schema di database (`prisma/schema.prisma`).

La metrica **LOC** rappresenta le linee totali del file (inclusi commenti e righe vuote), mentre la metrica **NCLOC** (Non-Comment Lines of Code) rappresenta le righe contenenti codice sorgente effettivo (escludendo righe vuote e linee interamente dedicate a commenti a riga singola o a blocco).

### Tabella riassuntiva delle dimensioni per file

| Nome File | LOC | NCLOC | Commenti / Righe Vuote |
| :--- | :---: | :---: | :---: |
| `src/modules/predictions/predictions.service.ts` | 354 | 280 | 74 |
| `src/modules/event/event.service.ts` | 231 | 169 | 62 |
| `src/modules/session/session.service.ts` | 209 | 168 | 41 |
| `src/modules/auth/auth.service.ts` | 159 | 122 | 37 |
| `src/modules/leaderboard/leaderboard.service.ts` | 110 | 82 | 28 |
| `src/server.ts` | 103 | 43 | 60 |
| `src/modules/event/event.controller.ts` | 91 | 76 | 15 |
| `src/modules/predictions/predictions.controller.ts` | 76 | 63 | 13 |
| `src/modules/auth/auth.controller.ts` | 73 | 53 | 20 |
| `src/middlewares/error.middleware.ts` | 63 | 50 | 13 |
| `src/modules/session/session.controller.ts` | 63 | 50 | 13 |
| `src/modules/event/event.routes.ts` | 55 | 44 | 11 |
| `src/utils/prisma.ts` | 41 | 5 | 36 |
| `src/utils/jwt.ts` | 35 | 29 | 6 |
| `src/middlewares/auth.middleware.ts` | 35 | 24 | 11 |
| `src/modules/session/session.routes.ts` | 35 | 27 | 8 |
| `src/routes/index.ts` | 33 | 27 | 6 |
| `src/modules/predictions/predictions.routes.ts` | 31 | 15 | 16 |
| `src/modules/event/event.validation.ts` | 29 | 24 | 5 |
| `src/app.ts` | 27 | 17 | 10 |
| `src/modules/auth/auth.validation.ts` | 21 | 17 | 4 |
| `src/middlewares/roles.middleware.ts` | 19 | 16 | 3 |
| `src/modules/leaderboard/leaderboard.controller.ts` | 18 | 14 | 4 |
| `src/modules/auth/auth.routes.ts` | 17 | 11 | 6 |
| `src/utils/response.ts` | 16 | 15 | 1 |
| `src/utils/bcrypt.ts` | 14 | 11 | 3 |
| `src/modules/session/session.validation.ts` | 14 | 11 | 3 |
| `src/types/express.d.ts` | 13 | 12 | 1 |
| `src/modules/predictions/predictions.validation.ts` | 11 | 8 | 3 |
| `src/modules/leaderboard/leaderboard.routes.ts` | 10 | 6 | 4 |
| **TOTALE** | **2006** | **1544** | **462** |

---

## 2. Complessità Ciclomatica (McCabe)

La Complessità Ciclomatica ($M$) misura la quantità di percorsi linearmente indipendenti attraverso il flusso di controllo del codice. Viene calcolata come:
$$M = 1 + d$$
dove $d$ rappresenta il numero di punti di decisione (come `if`, `for`, `while`, `catch`, operatori ternari `?` e operatori logici short-circuit `&&`, `||`, `??`).

### Valori medi del progetto
* **Complessità media di file/modulo** (top-level scope, al netto delle funzioni nidificate): **1.27**
* **Complessità media per funzione/metodo**: **1.96**

### Le funzioni/endpoint a complessità più elevata

Di seguito vengono dettagliate le 5 funzioni con il valore numerico di McCabe più alto a causa di elevata nidificazione di costrutti condizionali e cicli:

1. **`getLeaderboard()`** in `src/modules/leaderboard/leaderboard.service.ts`
   * **Valore McCabe:** **8**
   * **Linee:** 6-109
   * **Descrizione del Flusso:** Esegue il recupero degli utenti, il filtraggio dell'array per ruolo, un ciclo `for` di iterazione su ciascun utente, query nidificate per predizioni, un secondo ciclo condizionale per il calcolo del punteggio cumulativo della sessione, una mappatura di precisione con ordinamento dinamico e una mappatura finale per l'assegnazione dei rank.

2. **`authMiddleware()`** in `src/middlewares/auth.middleware.ts`
   * **Valore McCabe:** **6**
   * **Linee:** 5-35
   * **Descrizione del Flusso:** Gestisce le verifiche dell'header di autorizzazione, il token in formato Bearer, la decodifica, le eccezioni e l'assegnazione dell'utente all'oggetto di richiesta, gestendo svariati casi di errore (`AppError`).

3. **`errorHandler()`** in `src/middlewares/error.middleware.ts`
   * **Valore McCabe:** **6**
   * **Linee:** 17-63
   * **Descrizione del Flusso:** Rileva la tipologia di eccezione intercettata nei controller (istanza di `AppError`, errore di validazione `ZodError`, token scaduto `TokenExpiredError`, `JsonWebTokenError` o generico errore interno 500) operando diramazioni decisionali multiple.

4. **`savePredictions()`** in `src/modules/predictions/predictions.service.ts`
   * **Valore McCabe:** **6**
   * **Linee:** 72-143
   * **Descrizione del Flusso:** Valida lo stato della sessione attiva (stato `OPEN`), confronta la quantità degli ID evento inoltrati, controlla l'esistenza delle corrispondenze a DB, verifica l'assenza di duplicati con un oggetto `Set` ed esegue una transazione atomica Prisma.

5. **`addPrediction()`** in `src/modules/predictions/predictions.service.ts`
   * **Valore McCabe:** **6**
   * **Linee:** 188-271
   * **Descrizione del Flusso:** Controlla sessione e stato `OPEN`, verifica l'esistenza dell'evento specifico, conta le predizioni esistenti dell'utente a DB e confronta il limite consentito, verifica l'eventuale esistenza di una predizione pregressa ed esegue la scrittura su database.

---

## 3. Accoppiamento (Coupling & Information Flow)

Per valutare lo scambio informativo tra i moduli del sistema sono state misurate le seguenti metriche:
* **Fan-in:** Numero di moduli interni al progetto che importano/dipendono dal modulo analizzato.
* **Fan-out (Interno):** Numero di altri moduli interni al progetto importati dal modulo analizzato.
* **Fan-out (Esterno):** Numero di librerie esterne (`node_modules`) importate dal modulo analizzato.
* **Henry-Kafura Structural Complexity ($C_{HK}$):** Calcolata sia sul flusso informativo interno al sistema, sia sul flusso esteso alle librerie esterne tramite la formula:
  $$C_{HK} = (Fan\text{-}in \times Fan\text{-}out)^2$$

### Analisi dei moduli principali

La tabella mostra i dati calcolati per i 5 moduli di logica di business principali del backend e i 2 moduli di utilità/middleware a più alto impatto di accoppiamento.

| Nome File | Fan-in | Fan-out (Int) | Fan-out (Est) | Fan-out (Tot) | $C_{HK}$ (Solo Interno) | $C_{HK}$ (Flusso Totale) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| `src/modules/predictions/predictions.service.ts` | 1 | 3 | 0 | 3 | **9** | **9** |
| `src/modules/event/event.service.ts` | 1 | 3 | 0 | 3 | **9** | **9** |
| `src/modules/session/session.service.ts` | 1 | 3 | 1 | 4 | **9** | **16** |
| `src/modules/auth/auth.service.ts` | 1 | 5 | 0 | 5 | **25** | **25** |
| `src/modules/leaderboard/leaderboard.service.ts` | 1 | 2 | 0 | 2 | **4** | **4** |
| `src/utils/prisma.ts` | 6 | 0 | 1 | 1 | **0** | **36** |
| `src/middlewares/error.middleware.ts` | 6 | 1 | 2 | 3 | **36** | **324** |

### Tipologie di Accoppiamento Rilevate

1. **Common Coupling (Accoppiamento Comune):**
   * **Database Centralizzato:** Tutti e 5 i servizi applicativi importano e utilizzano la medesima istanza del client Prisma dal file `src/utils/prisma.ts`. Questo introduce accoppiamento globale sullo stato del database. Se lo schema di Prisma o le opzioni di connessione mutano, tutti i moduli subiscono un impatto diretto.
   * **Variabili d'Ambiente:** Più moduli leggono dallo stato globale di configurazione fornito da `process.env` (`PORT`, `JWT_SECRET`, `JWT_EXPIRES_IN`, `CORS_ORIGIN`).
2. **Content Coupling (Accoppiamento di Contenuto):**
   * **Non rilevato.** Il codice rispetta i confini di visibilità e astrazione. L'interazione tra i controller e i rispettivi servizi avviene esclusivamente mediante la chiamata a metodi pubblici delle classi istanziate, impedendo modifiche dirette dello stato interno o del codice esecutivo.

---

## 4. Coesione (Cohesion)

La classificazione della coesione valuta il grado di correlazione tra le responsabilità e le funzioni esposte da ciascun modulo.

### Analisi dei 3 file/moduli più grandi del progetto

#### 1. `src/modules/predictions/predictions.service.ts` (280 NCLOC)
* **Classificazione:** **Coesione Comunicazionale (Communicational Cohesion)**
* **Motivazione:** Le funzioni presenti (`getMyPredictions`, `savePredictions`, `removePrediction`, `addPrediction`, `getPredictionsStats`, `hasCompletedPredictions`) non eseguono un singolo compito, ma condividono la medesima struttura dati di input/output (le tabelle di Prisma `Prediction` e `GameSession`) ed i parametri centrali (come il parametro identificativo `userId`). Le operazioni sono raggruppate per risorsa DB di riferimento.

#### 2. `src/modules/event/event.service.ts` (169 NCLOC)
* **Classificazione:** **Coesione Comunicazionale (Communicational Cohesion)**
* **Motivazione:** Tutte le funzioni cooperano manipolando lo stato degli eventi associati a una sessione di gioco (`getEvents`, `createEvent`, `updateEvent`, `deleteEvent`, `verifyEvent`, `verifyBatch`, `reorderEvents`). I metodi agiscono sulla stessa tabella relazionale (`Event`) condividendo logiche di validazione comuni a livello di transazioni.

#### 3. `src/modules/session/session.service.ts` (168 NCLOC)
* **Classificazione:** **Coesione Comunicazionale (Communicational Cohesion)**
* **Motivazione:** Il file aggrega le procedure legate al ciclo di vita di una sessione (`getCurrentSession`, `createSession`, `updateSessionState`, `resetSession`, `getSessionStats`) e la validazione privata delle transizioni di stato. L'interazione avviene in via esclusiva sui record del modello `GameSession`.

### Contrasto con Moduli a Coesione Funzionale

A titolo comparativo per lo studio accademico, il modulo **`src/modules/leaderboard/leaderboard.service.ts`** presenta un livello di **Coesione Funzionale (Functional Cohesion)**, il gradino più alto della scala standard. Esso racchiude un'unica funzione pubblica (`getLeaderboard()`) preposta all'esecuzione di una singola operazione matematica e di calcolo computazionale preciso (classifica di merito finale basata sull'accuracy degli utenti).

---

## 5. Duplicazione del Codice (Code Clones)

L'analisi dei cloni di codice ha esaminato la ridondanza sintattica per porzioni di codice consecutive con una finestra minima di $W = 6$ righe NCLOC.

### Metriche di Ridondanza
* **Righe totali ridondanti identificate:** **976 NCLOC**
* **Percentuale di ridondanza stimata:** **63.21%**

L'elevata percentuale è dovuta principalmente all'adozione sistematica di boilerplate ripetitivo all'interno delle classi Controller di Express (che avvolgono le chiamate ai metodi del Service in blocchi `try-catch` con inoltro standard degli errori a `next(error)`) e a logiche ripetute di recupero e controllo delle sessioni attive all'inizio delle procedure di business.

### Cloni di Tipo-1 (Copia-incolla identico)

Si segnalano le seguenti aree con blocchi di codice testualmente sovrapponibili:

* **Gestione dei blocchi di controllo ed esistenza sessione (`src/modules/event/event.service.ts`):**
  Identico blocco a 6 linee ripetuto alle righe 35-43, 73-81 e 106-114:
  ```typescript
  const session = await prisma.gameSession.findFirst();
  if (!session) {
    throw new AppError(404, 'No active session found', 'NO_SESSION');
  }
  if (session.state !== 'SETUP') {
  ```

* **Estrazione campi utente per queries SELECT (`src/modules/auth/auth.service.ts`):**
  Blocco identico per la selezione dei campi in Prisma, ripetuto alle righe 30-35 e 144-149:
  ```typescript
  select: {
    id: true,
    email: true,
    username: true,
    role: true,
    createdAt: true,
  }
  ```

* **Struttura di ritorno dei Token di Sessione (`src/modules/auth/auth.service.ts`):**
  Ripetuto specularmente alle righe 40-50 e 78-88:
  ```typescript
  const accessToken = generateAccessToken({
    userId: user.id,
    email: user.email,
    role: user.role,
  });
  const refreshToken = generateRefreshToken({
  ```

### Cloni di Tipo-2 (Variazioni sintattiche o rinomina variabili)

I cloni di Tipo-2 presentano medesima struttura di controllo ma operano con parametri o invocazioni di metodi con nomi alternativi:

* **Modello dei controller per le risposte HTTP (`src/modules/predictions/predictions.controller.ts`):**
  Struttura metodologica analoga rilevata in tre blocchi del file (righe 25-30, 37-42 e 49-54):
  ```typescript
  // La struttura astratta normalizzata è identica:
  const ID_VAL = await ID_SERVICE.ID_METHOD(ID_PARAM1, ID_PARAM2);
  res.status(NUM).json(successResponse(ID_VAL));
  } catch (error) {
    next(error);
  }
  ```

* **Logica di verifica stato transazioni (`src/modules/event/event.service.ts` e `src/modules/session/session.service.ts`):**
  Le logiche di validazione temporale ed eccezione di stato della sessione (righe 114-119, 146-151, 179-184 in `event.service.ts` e righe 67-72 in `session.service.ts`) condividono la medesima struttura normalizzata:
  ```typescript
  if (session.state !== 'STR') {
    throw new AppError(
      NUM,
      'STR',
      'STR'
    );
  }
  ```
| Metrica Analizzata | PredictGame (Prompt Libero) | NetScore26 (Prompt Architetturale) | L'Evidenza |
| :--- | :--- | :--- | :--- |
| **Dimensione (NCLOC)** | 1544 | 1109 | L'AI non vincolata genera verbosità. |
| **Duplicazione (Cloni)** | **63.21%** | **6.5%** | Senza guida, l'AI risolve i task copiando e incollando. |
| **McCabe Medio** | 1.27 | 7.13 | Un McCabe fittiziamente basso maschera l'assenza di astrazione. |
| **McCabe Massimo** | 8 | **19** | Centralizzare la logica svela il limite dell'AI sugli algoritmi complessi. |
| **Organizzazione** | Boilerplate Ripetitivo | Astrazione e Centralizzazione | Il prompt definisce se l'AI è un *code monkey* o un progettista. |