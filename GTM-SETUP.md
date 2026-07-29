# GTM nastavenie — Meta Pixel eventy z quizu

Web posiela udalosti do `dataLayer`. V GTM (`GTM-TMLKGT95`) nastav tagy, ktoré ich odošlú do Meta Pixelu (`1402052265114481`).

## Udalosti z webu

| dataLayer event | Kedy | Meta event |
|-----------------|------|------------|
| `Q1_sent` | Odpoveď na otázku 1 | Custom: `Q1_sent` |
| `Q2_sent` | Odpoveď na otázku 2 | Custom: `Q2_sent` |
| `Q3_sent` | Odpoveď na otázku 3 | Custom: `Q3_sent` |
| `Q4_sent` | Odpoveď na otázku 4 (vrátane „Iné“) | Custom: `Q4_sent` |
| `form_start` | Prvé písanie do kontaktného formulára | Custom: `FormStart` |
| `form_submit` | Úspešné odoslanie | Standard: `Lead` |

Každá udalosť max **1× za 30 minút** (sessionStorage) — aj pri „Späť“.

Voliteľné parametre v dataLayer (bez PII): `quiz_question`, `quiz_answer`, `source`.

## Kroky v GTM

### 1. Data Layer Variables (voliteľné)

V **Variables → New → Data Layer Variable**:

- `quiz_question` — Data Layer Variable Name: `quiz_question`
- `quiz_answer` — Data Layer Variable Name: `quiz_answer`

### 2. Triggery

Pre každú udalosť: **Triggers → New → Custom Event**

| Trigger name | Event name |
|--------------|------------|
| CE - Q1_sent | `Q1_sent` |
| CE - Q2_sent | `Q2_sent` |
| CE - Q3_sent | `Q3_sent` |
| CE - Q4_sent | `Q4_sent` |
| CE - form_start | `form_start` |
| CE - form_submit | `form_submit` |

### 3. Meta Pixel tagy

**Tags → New → Facebook Pixel** (alebo Custom HTML s `fbq`)

Pre sekundárne udalosti (Q1–Q4, form_start):

- Tag type: Facebook Pixel — **Custom Event**
- Pixel ID: `1402052265114481`
- Event Name: podľa tabuľky vyššie
- Trigger: príslušný Custom Event

Pre konverziu:

- Tag: Facebook Pixel — **Standard Event**
- Event: `Lead`
- Trigger: `CE - form_submit`

### 4. PageView — duplicita

V `<head>` webu je Meta Pixel s `PageView` a v GTM tiež FB PageView tag.
**Odporúčanie:** nechaj PageView len na jednom mieste (ideálne GTM), druhé vypni — inak Meta počíta 2× page view.

### 5. Publikovanie a test

1. **Preview** v GTM
2. Prejdi quiz na live alebo localhost
3. V Preview over, že každý event prebehne max 1×
4. V **Meta Events Manager → Test Events** over príjem
5. **Submit** v GTM

## Meta Events Manager

Po publikovaní v GTM vytvor v Meta:

- Custom conversions pre `Q1_sent` … `Q4_sent`, `FormStart` (voliteľné)
- Hlavnú konverziu z eventu **Lead** pre optimalizáciu reklám
