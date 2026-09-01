# Excalibur Festas e Eventos — LP

Landing page de pré-qualificação para campanha de Meta Ads. Mobile-first,
HTML único + assets estáticos, sem build step — deploy direto no Netlify.

📄 Documentação completa (arquitetura, tracking, pendências):
[`DOCUMENTATION.md`](../DOCUMENTATION.md) na raiz do handoff (ou peça pro
autor original se não estiver junto neste repo).

## Rodar localmente
```bash
python3 -m http.server 8080
# abre http://localhost:8080
```
Sem dependências, sem `npm install`.

## Deploy
Drag-and-drop da pasta (ou `netlify deploy --prod`) direto no Netlify.
Não há variáveis de ambiente nesta parte do projeto — os IDs (Pixel,
WhatsApp, Clarity, endpoint do worker) estão hardcoded no `index.html`
porque são todos client-side e não sensíveis.

## Repositório irmão
O worker de Conversions API (`excalibur-capi`) fica em repositório
separado — ver `DOCUMENTATION.md` seção 6.

## Padrão: captura de atribuição (fbclid, UTMs, campaign/adset/ad id)

**Este é o padrão a ser replicado em toda nova LP.** No `index.html`, antes
de `sendServerEvent`, existe `captureAttribution()`: lê da URL os
parâmetros `fbclid`, `utm_source`, `utm_medium`, `utm_campaign`,
`utm_content`, `utm_term`, `campaign_id`, `adset_id`, `ad_id` e persiste em
`sessionStorage` (chave `xlb_attr`) — assim eles sobrevivem a navegações
dentro da própria LP, mesmo se o usuário sair da URL original com os
parâmetros.

Esse objeto é enviado em todo evento pro Worker (`sendServerEvent`), que
repassa como `custom_data` no payload da Conversions API — ficando visível
no Events Manager e permitindo cruzar lead × anúncio/campanha nos
relatórios (ver `excalibur-capi/worker.js`).

Para os campos `campaign_id`, `adset_id` e `ad_id` chegarem preenchidos,
os anúncios no Meta Ads Manager precisam ter parâmetros de URL configurados
(aba "Rastreamento" do anúncio), por exemplo:
```
utm_source=facebook&utm_medium=paid&utm_campaign={{campaign.name}}&campaign_id={{campaign.id}}&adset_id={{adset.id}}&ad_id={{ad.id}}
```
`fbclid` já é anexado automaticamente pela Meta na maioria dos casos, sem
precisar configurar nada.
