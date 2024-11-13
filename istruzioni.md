# Guida Implementazione Modello Iubenda "Forced"

## Panoramica
Questa guida descrive il processo di migrazione a un modello "forced" di iubenda, dove analytics_storage è sempre consentito di default per ottimizzare la raccolta dati mantenendo la conformità attraverso l'anonimizzazione PII.

## 1. Pulizia Container Esistente

### 1.1 Rimozione Bulk Eccezioni
- Rimuovere tutte le eccezioni "No Hit" dai tag
- Rimuovere tutte le eccezioni "URL Query No Hit"
- 

### 1.2 Rimozione Tag
- Tag iubenda esistente
- Tag relativi al consenso
- Tag Google Analytics esistente

### 1.3 Pulizia Attivatori
- Tutti gli attivatori legati a iubenda
- Tutti gli attivatori legati al consenso

### 1.4 Pulizia Variabili
**Mantenere:**
- CookiePolicy ID e variabile di gestione correlata
- Site ID
- Gestione Lingua e variabili collegate

**Eliminare:**
- Tutte le altre variabili legate a iubenda

## 2. Importazione Nuovo Container

### 2.1 Processo di Import
1. Amministra > Importa Container
2. Selezionare file iubenda.json
3. Scegliere modalità "Unisci"
4. Risolvere eventuali conflitti di nome

### 2.2 Configurazione ID Monitoraggio
Sostituire in tutti i tag necessari:
`{{ga4 code}}` -> `{{DEXA - GA4 - ID Misurazione}}`

## 3. Migrazione Configurazioni

### 3.1 Trasferimento Valori
Trasferire i valori dalle vecchie alle nuove variabili:
- DEXA - Iubenda - SiteID           <- vecchio Site ID
- DEXA - Iubenda - CookiePolicyID   <- vecchio Cookie Policy ID
- DEXA - Iubenda - Gestione Lingua  <- vecchia gestione lingua
- DEXA - Iubenda - Background bottoni
- DEXA - Iubenda - Colore Testo Bottoni

### 3.2 Verifica Configurazioni
- Controllare correttezza ID trasferiti
- Verificare mappatura lingue
- Confermare personalizzazioni colori banner

## 4. Testing

### 4.1 Verifica Funzionale
1. Attivare Preview Mode
2. Controllare:
   - Apparizione banner
   - Funzionamento pulsanti consenso
   - Persistenza preferenze

### 4.2 Verifica Analytics
Controllare:
- Tracking analytics attivo di default
- Anonimizzazione PII funzionante
- Eventi tracciati correttamente

### 4.3 Verifica Consenso
Verificare stato consenso in:
- Chrome DevTools > Application > Cookies
- Debug Console GTM
- Real-time GA4

## 5. Pubblicazione

### 5.1 Pre-pubblicazione
- Documentare tutte le modifiche
- Verificare funzionamento in preview
- Controllare tutti i consensi

### 5.2 Go-Live
1. Creare nuova versione
2. Pubblicare container
3. Monitorare analytics per 24-48h

## Note Tecniche
- Il modello "forced" mantiene analytics_storage sempre attivo
- L'anonimizzazione PII viene gestita separatamente
- Configurazione ottimizzata per massimizzare la raccolta dati mantenendo GDPR compliance

## Troubleshooting

### Problemi Comuni

1. **Analytics non traccia:**
   - Verificare ID Misurazione
   - Controllare trigger All Pages

2. **Banner non appare:**
   - Verificare Site ID
   - Controllare Cookie Policy ID
   - Verificare trigger Consent Initialization

3. **Problemi consenso:**
   - Pulire cache browser
   - Verificare cookie _iub_cs
   - Controllare Consent Mode in debug
