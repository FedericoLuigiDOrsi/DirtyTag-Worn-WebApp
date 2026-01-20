# Vinted Worn Selector

Webapp per selezionare items e generare foto "worn style" via AI per Vinted.

## 🎯 Funzionalità

- Visualizzazione grid items eleggibili per foto worn
- Filtri per SKU, Brand, Category, Gender, Taglia
- Selezione multipla (max 50 items per batch)
- Configurazione pose da generare (1-5)
- Invio batch a n8n per elaborazione AI

## 📋 Requisiti

- Browser moderno (Chrome, Firefox, Safari, Edge)
- Airtable Personal Access Token con accesso alla base DirtyTag 3.0

## 🚀 Setup su GitHub Pages

### 1. Fork/Clone Repository

```bash
git clone https://github.com/TUO_USERNAME/vinted-selector.git
cd vinted-selector
```

### 2. Abilita GitHub Pages

1. Vai su **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** / **root**
4. Salva

### 3. Attendi Deploy

Il deploy richiede 1-2 minuti. L'URL sarà:
```
https://TUO_USERNAME.github.io/vinted-selector
```

### 4. Configura API Key

Al primo accesso:
1. Modal configurazione appare automaticamente
2. Inserisci Airtable API Key
3. Webhook URL è pre-configurato (modificabile se necessario)
4. Salva

## 🔑 Ottenere Airtable API Key

1. Vai su [airtable.com/create/tokens](https://airtable.com/create/tokens)
2. Crea nuovo token
3. **Scope richiesto:** `data.records:read`
4. **Base access:** DirtyTag 3.0 (AI)
5. Copia il token (inizia con `pat...`)

## 📖 Utilizzo

### Workflow Operativo

1. **Filtra** items usando search e dropdown
2. **Seleziona** items cliccando sulle card (max 50)
3. **Configura** pose da generare (default: tutte)
4. **Seleziona** modello (Auto consigliato)
5. Click **"GENERA FOTO WORN"**
6. Attendi conferma
7. **Apri Review** per approvare le foto generate

### Eleggibilità Items

Un item è eleggibile per foto worn se:
- ✅ Ha foto AI Front e Back generate
- ✅ Ha gender specificato
- ✅ Non è già stato venduto (SOLD)

### Pose Disponibili

| # | Descrizione |
|---|-------------|
| 1 | Mirror selfie frontale |
| 2 | Mirror selfie angolato |
| 3 | Candid vista retro |
| 4 | Candid profilo laterale |
| 5 | Detail close-up |

### Selezione Modello

| Opzione | Quando usare |
|---------|--------------|
| **Auto** | Scelta automatica basata su gender + size (consigliato) |
| Female Size S | Force modella taglia S |
| Female Size M | Force modella taglia M |
| Male | Force modello maschile |

## 🔧 Troubleshooting

### Errore 401 Unauthorized
- API key non valida o scaduta
- Rigenerare token su Airtable

### Errore 403 Forbidden
- Token non ha accesso alla base
- Verificare scope e base access nelle impostazioni token

### Nessun item visibile
- Verificare che ci siano items eleggibili in Airtable
- Controllare che abbiano foto AI e gender

### Webhook non risponde
- Verificare che il workflow n8n sia attivo
- Controllare l'URL webhook nella configurazione

## 📁 Struttura Files

```
vinted-selector/
├── index.html     # Webapp (tutto inline)
├── README.md      # Questa guida
└── .nojekyll      # Per GitHub Pages
```

## ⚙️ Configurazione Avanzata

### Cambiare Webhook URL

1. Click icona ⚙️ in alto a destra
2. Modifica campo "Webhook URL"
3. Salva

### Reset Configurazione

```javascript
// In console browser
localStorage.removeItem('vinted_selector_api_key');
localStorage.removeItem('vinted_selector_webhook');
```

## 📊 Schema Airtable Usato

### Campi Letti da INVENTARIO

| Campo | Tipo | Note |
|-------|------|------|
| SKU | Text | Identificatore |
| Brand_TXT | Formula | Mirror di Brand |
| Category_TXT | Formula | Mirror di Category |
| Sub-Category_TXT | Formula | Mirror di Sub-Category |
| gender | Single Select | Men/Women/Unisex/Kids |
| Tag Size | Text | Taglia etichetta |
| Colors_TXT | Formula | Mirror di Colors |
| Condizione | Single Select | Stato conservazione |
| AI_Front_Image_Link | URL | Foto AI front |
| AI_Back_Image_Link | URL | Foto AI back |
| € Iniziale | Number | Prezzo listing |
| Product_Status | Single Select | Per escludere SOLD |

## 🔗 Integrazione n8n

La webapp invia richieste POST al webhook con questo payload:

```json
{
  "request_id": "req_1705678234567_abc123",
  "items": ["recXXX", "recYYY", "recZZZ"],
  "config": {
    "model_selection": "auto",
    "poses": [1, 2, 3, 4, 5],
    "quality_threshold": "standard"
  },
  "created_by": "webapp_operator",
  "timestamp": "2025-01-19T14:30:00.000Z"
}
```

---

## 📝 Changelog

### v1.0.0 (2026-01-20)
- Release iniziale
- Filtri per SKU, Brand, Category, Gender, Size
- Selezione multipla (max 50)
- Integrazione webhook n8n
- Eleggibilità client-side (no dipendenza da campi Airtable inesistenti)

---

**DirtyTag 3.0** — Sistema AI per vintage fashion e-commerce
