# Guida Implementazione Modello Iubenda "Forced"

## Panoramica
Questa guida descrive il processo di migrazione a un modello "forced" di iubenda, dove analytics_storage è sempre consentito di default per ottimizzare la raccolta dati mantenendo la conformità attraverso l'anonimizzazione PII.

## 1. Pulizia Container Esistente

### 1.1 Rimozione Bulk Eccezioni
- Rimuovere tutte le eccezioni "No Hit" dai tag
- Rimuovere tutte le eccezioni "URL Query No Hit"
- Verificare che non ci siano eccezioni residue nei tag

![Rimozione Attributi](./immagini/rimozione%20attributi.png)

*Esempio di rimozione delle eccezioni bulk dai tag*

### 1.2 Rimozione Tag
- Tag iubenda esistente
- Tag relativi al consenso
- Tag Google Analytics esistente
- Assicurarsi di non rimuovere tag business-critical

![Rimozione Tag](./immagini/rimozione%20tag.png)

*Panoramica dei tag da rimuovere*

### 1.3 Pulizia Attivatori
- Tutti gli attivatori legati a iubenda
- Tutti gli attivatori legati al consenso
- Verificare le dipendenze prima della rimozione

![Rimozione Attivatori](./immagini/rimozione%20attivatori.png)

*Lista degli attivatori da rimuovere*

### 1.4 Pulizia Variabili
**Mantenere:**
- CookiePolicy ID e variabile di gestione correlata
- Site ID
- Gestione Lingua e variabili collegate

**Eliminare:**
- Tutte le altre variabili legate a iubenda
- Verificare le dipendenze prima della rimozione

![Rimozione Variabili](./immagini/rimozione%20variabili.png)

*Esempio di variabili da mantenere e rimuovere*

## 2. Importazione Nuovo Container

### 2.1 Processo di Import
1. Accedere a Amministra > Importa Container
2. Selezionare il file iubenda.json fornito
3. Scegliere la modalità "Unisci"
4. Gestire eventuali conflitti di nome mantenendo le variabili esistenti dove necessario

![Importazione Modello](./immagini/importazione%20modello.png)

*Schermata di importazione del container*

### 2.1.1 Attivazione Panoramica Consenso
Per gestire efficacemente i tag relativi al consenso:
1. Accedere alle impostazioni del container
2. Attivare l'opzione "Attiva panoramica del consenso"
3. Salvare le modifiche

![Attivazione Panoramica Consenso](./immagini/attivazione%20panoramica%20del%20consenso.png)

*Attivazione della panoramica consenso nelle impostazioni*

### 2.2 Configurazione ID Monitoraggio e Variabili

#### 2.2.1 Migrazione GA4
1. Identificare tutti i tag che utilizzano `{{ga4 code}}`
2. Sostituire con `{{DEXA - GA4 - ID Misurazione}}`
3. Verificare la corretta propagazione dell'ID

![Migrazione GA4](./immagini/migrazione%20codice%20ga4.png)

*Processo di sostituzione dell'ID GA4*

#### 2.2.2 Aggiornamento CookiePolicy ID
1. Localizzare la variabile CookiePolicy ID esistente
2. Effettuare la migrazione alla nuova variabile DEXA
3. Verificare la corretta associazione

![Migrazione CookiePolicy](./immagini/migrazione%20gestione%20cookiepolicyID.png)

*Processo di migrazione del CookiePolicy ID*

#### 2.2.3 Configurazione Lingua
1. Accedere al tag HTML di iubenda
2. Sostituire la vecchia variabile lingua
3. Implementare la nuova variabile di gestione lingua
4. Verificare il corretto funzionamento multilingua

![Azione Edit HTML](./immagini/azione%20dello%20scambiare%20variabile%20vecchia%20nel%20nuovo%20tag%20iubenda%20per%20la%20lingua.png)

*Accesso alla configurazione del tag HTML di iubenda*

![Gestione Lingua](./immagini/scambiare%20nel%20tag%20HTML%20iubenda%20la%20variabile%20lingua%20con%20quella%20presente%20nel%20contenitore.png)

*scambiamo i nomi delle variabili ed eliminiamo la variabile inutilizzata*

![Controllo Gestione Lingua](./immagini/dopo%20la%20modifica%20della%20variabile%20lingua.png)

*Verifica della corretta implementazione della gestione lingua*

## 3. Migrazione Configurazioni

### 3.1 Trasferimento Valori
Eseguire la migrazione dei seguenti valori:
- DEXA - Iubenda - SiteID           <- vecchio Site ID
- DEXA - Iubenda - CookiePolicyID   <- vecchio Cookie Policy ID
- DEXA - Iubenda - Gestione Lingua  <- vecchia gestione lingua
- DEXA - Iubenda - Background bottoni
- DEXA - Iubenda - Colore Testo Bottoni

### 3.2 Verifica Configurazioni
1. Controllare la correttezza degli ID trasferiti
2. Verificare la mappatura delle lingue
3. Confermare le personalizzazioni dei colori del banner
4. Testare il funzionamento multilingua

## 4. Testing

### 4.1 Verifica Funzionale
1. Attivare la Preview Mode
2. Verificare:
   - Corretta apparizione del banner
   - Funzionamento dei pulsanti di consenso
   - Persistenza delle preferenze utente

### 4.2 Verifica Analytics
Eseguire i seguenti controlli:
1. Verificare che il tracking analytics sia attivo di default
2. Controllare il funzionamento dell'anonimizzazione PII
3. Verificare il corretto tracking degli eventi

![Controllo Measurement ID](./immagini/controlliamo%20sia%20corretto%20il%20measurement%20ID%20di%20Ga4.png)

*Verifica del Measurement ID GA4*

### 4.3 Verifica Consenso
Controllare lo stato del consenso in:
1. Chrome DevTools > Application > Cookies
2. Debug Console GTM
3. Real-time GA4

![Verifica Consenso](./immagini/corretto%20stato%20del%20consenso%20banner%20non%20accettato.png)

*Esempio di stato corretto del consenso*

## 5. Verifica Stato Finale

### 5.1 Controlli di Sanità
Verificare la corretta configurazione di:

#### Attivatori
![Stato Attivatori](./immagini/esempio%20stato%20sano%20sezione%20attivatori.png)

*Configurazione corretta degli attivatori*

#### Tag
![Stato Tag](./immagini/esempio%20stato%20sano%20sezione%20tag.png)

*Configurazione corretta dei tag*

#### Variabili
![Stato Variabili](./immagini/esempio%20stato%20sano%20variabili%20personalizzate.png)

*Configurazione corretta delle variabili*

## 6. Pubblicazione e Monitoraggio

### 6.1 Pre-pubblicazione
1. Documentare tutte le modifiche effettuate
2. Verificare il funzionamento in preview
3. Controllare tutti i consensi
4. Eseguire test su diversi browser

### 6.2 Go-Live
1. Creare una nuova versione del container
2. Pubblicare il container
3. Monitorare analytics per 24-48h
4. Verificare l'assenza di errori in console

## Note Tecniche
- Il modello "forced" mantiene analytics_storage sempre attivo
- L'anonimizzazione PII viene gestita separatamente
- La configurazione è ottimizzata per massimizzare la raccolta dati mantenendo la GDPR compliance
- Verificare regolarmente gli aggiornamenti di iubenda

## Troubleshooting

### Problemi Comuni

1. **Analytics non traccia:**
   - Verificare ID Misurazione
   - Controllare trigger All Pages
   - Verificare la presenza di blocchi browser

2. **Banner non appare:**
   - Verificare Site ID
   - Controllare Cookie Policy ID
   - Verificare trigger Consent Initialization
   - Controllare conflitti JavaScript

3. **Problemi consenso:**
   - Pulire cache browser
   - Verificare cookie _iub_cs
   - Controllare Consent Mode in debug
   - Verificare la corretta implementazione multilingua
