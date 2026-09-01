# GTM nastavenie — Meta Pixel eventy z formulára

Web posiela udalosti do `dataLayer`. V GTM (`GTM-TMLKGT95`) nastav tagy, ktoré ich odošlú do Meta Pixelu (`1402052265114481`).

Quiz eventy `Q1_sent` až `Q4_sent` sa už neposielajú. Stránka má klasický kontaktný formulár.

## Udalosti z webu

| dataLayer event | Kedy | Meta event |
|-----------------|------|------------|
| `form_start` | Prvé písanie do formulára | Custom: `FormStart` |
| `form_submit` | Úspešné odoslanie | Standard: `Lead` |

Každá udalosť max **1× za 30 minút** (sessionStorage).

Voliteľný parameter v dataLayer (bez PII): `source`.

## Kroky v GTM

### 1. Triggery

Pre každú udalosť: **Triggers → New → Custom Event**

| Trigger name | Event name |
|--------------|------------|
| CE - form_start | `form_start` |
| CE - form_submit | `form_submit` |

### 2. Meta Pixel tagy

**Tags → New → Facebook Pixel** (alebo Custom HTML s `fbq`)

Pre `form_start`:

- Tag type: Facebook Pixel — **Custom Event**
- Pixel ID: `1402052265114481`
- Event Name: `FormStart`
- Trigger: `CE - form_start`

Pre konverziu:

- Tag: Facebook Pixel — **Standard Event**
- Event: `Lead`
- Trigger: `CE - form_submit`

### 3. PageView — duplicita

V `<head>` webu je Meta Pixel s `PageView` a v GTM tiež FB PageView tag.
**Odporúčanie:** nechaj PageView len na jednom mieste (ideálne GTM), druhé vypni — inak Meta počíta 2× page view.

### 4. Publikovanie a test

1. **Preview** v GTM
2. Vyplň formulár na live alebo localhost
3. V Preview over, že `form_start` a `form_submit` prebehnú max 1×
4. V **Meta Events Manager → Test Events** over príjem
5. **Submit** v GTM

## Meta Events Manager

Po publikovaní v GTM:

- Custom conversion pre `FormStart` (voliteľné)
- Hlavnú konverziu z eventu **Lead** pre optimalizáciu reklám
