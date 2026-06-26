# 📊 L'Impatto del Prompting Architetturale sull'AI Generativa
**Report di Analisi Statica: PredictGame vs NetScore26**

Questa repository contiene i dati grezzi e i log integrali dell'analisi statica condotta per lo studio comparativo sulla qualità del codice generato tramite Large Language Models (LLM). 

L'obiettivo dello studio è dimostrare empiricamente la differenza qualitativa tra il codice generato tramite *Vibecoding* (Prompting Libero) e quello generato tramite *Prompting Architetturale* (vincolato a design pattern rigorosi).

---

## 📝 Descrizione dello Studio

Con la crescente adozione dell'AI generativa nello sviluppo software, emerge il rischio di accumulare rapidamente debito tecnico se le macchine vengono lasciate operare senza vincoli di design. 

Per quantificare questo fenomeno, sono stati generati due backend in **Node.js / Express.js / TypeScript**, affidando all'AI la stesura del codice sorgente ma variando radicalmente il metodo di interazione:

* 🔴 **PredictGame (Prompt Libero):** L'AI ha ricevuto esclusivamente i requisiti funzionali. Nessuna direttiva su astrazione, separazione delle responsabilità o architettura.
* 🟢 **NetScore26 (Prompt Architetturale):** L'AI è stata vincolata a rispettare un'impalcatura ingegneristica definita a priori, imponendo l'uso di pattern architetturali, middleware e logiche di servizio isolate.

Entrambe le codebase sono state sottoposte ad analisi statica sulla directory operativa (`src/`), escludendo librerie esterne e configurazioni per isolare il reale output della macchina.

---

## 🔬 Metodologia e Metriche Misurate

I report presenti in questa repository valutano lo stato di salute dei due sistemi basandosi su quattro pilastri dell'ingegneria del software:

1.  **Dimensione (NCLOC):** *Non-Comment Lines of Code*. Misura la verbosità e l'estensione della superficie di manutenzione.
2.  **Duplicazione (Code Clones):** Valutazione della ridondanza sintattica (Cloni di Tipo-1 e Tipo-2) e violazione del principio DRY (*Don't Repeat Yourself*).
3.  **Complessità Ciclomatica (McCabe):** Calcolo dei percorsi linearmente indipendenti ($M = 1 + d$) per misurare il rischio di difettosità e la difficoltà di testing.
4.  **Accoppiamento e Coesione:** Analisi del flusso informativo (tramite complessità strutturale di Henry-Kafura, $C_{HK}$) e valutazione della correlazione delle istruzioni all'interno dei moduli.

---

## 📂 Struttura della Repository

I risultati integrali dell'analisi sono suddivisi nei seguenti documenti Markdown:

* 📄 [`PredictGame_Analysis.md`](./Analisys.md) 
    *Report dettagliato del progetto generato con Prompt Libero. Evidenzia una duplicazione del codice superiore al 63% e l'emersione di boilerplate procedurale.*
* 📄 [`NetScore26_Analysis.md`](./Analysis.md) 
    *Report dettagliato del progetto guidato tramite Prompt Architetturale. Mostra un crollo della ridondanza al 6.5% e una corretta centralizzazione delle responsabilità.*

*(Nota: i nomi dei file riflettono i documenti originali allegati allo studio).*

---

## 🚀 Risultati in Breve

| Metrica Analizzata | PredictGame (Prompt Libero) | NetScore26 (Prompt Architetturale) |
| :--- | :--- | :--- |
| **Dimensione (NCLOC)** | 1544 | 1109 |
| **Duplicazione (Cloni)** | **63.21%** | **6.5%** |
| **McCabe Medio** | 1.27 | 7.13 |
| **McCabe Massimo** | 8 | 19 |

L'analisi dimostra che l'assenza di vincoli architetturali spinge l'AI generativa a risolvere i problemi in modo locale, procedurale e altamente ridondante, generando un debito tecnico insostenibile nel lungo periodo. Il ruolo del Software Engineer si evolve quindi nella "progettazione di vincoli" per incanalare correttamente la capacità computazionale degli LLM.

---
*Progetto di ricerca per studio accademico comparativo sull'Ingegneria del Software.*
