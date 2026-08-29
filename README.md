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
