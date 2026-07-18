# VitaIA — o que mudou

## 🔐 IA agora funciona de verdade (e com segurança)
O código antigo tentava chamar a API da Anthropic **direto do navegador**, com uma
chave falsa (`sk-ant-api03-placeholder`) — isso nunca funcionou. Pior: havia uma
**chave real da Groq exposta no código-fonte** (`gsk_exQME5...`), visível para
qualquer visitante do site. Eu removi essa chave e troquei toda a lógica por um
endpoint de backend (`/api/ai`) que guarda sua chave da Anthropic **só no servidor**.

Arquivos novos:
- `api/ai.js` → função serverless (Vercel) que fala com a Anthropic.
- `server.js` → alternativa em Express, caso você hospede em VPS/Railway/Render/etc.
- `package.json`, `vercel.json`, `.env.example` → configuração do backend.

## 🚀 Como colocar no ar

### Opção A — Vercel (mais simples)
1. Suba esta pasta para um repositório no GitHub.
2. Importe o repositório em [vercel.com](https://vercel.com/new).
3. Em **Settings → Environment Variables**, adicione:
   - `ANTHROPIC_API_KEY` = sua chave, gerada em https://console.anthropic.com/settings/keys
4. Deploy. O site e o endpoint `/api/ai` sobem juntos automaticamente.

### Opção B — Servidor próprio (Node)
```bash
npm install
cp .env.example .env       # edite e cole sua ANTHROPIC_API_KEY
npm start                   # roda em http://localhost:3000
```
Funciona em Render, Railway, um VPS, etc. — qualquer lugar que rode Node 18+.

> Sem `ANTHROPIC_API_KEY` configurada, a IA mostra um erro claro em vez de falhar
> silenciosamente, e cai no plano B da Groq caso o usuário tenha colado uma chave
> pessoal em `localStorage` (avançado/opcional).

## 🎨 Visual
O `style.css` recebeu uma nova paleta (mais rica, com gradientes violeta→rosa e
azul-índigo) e uma camada extra de polimento no final do arquivo: sombras em
camadas, cards com hover, botões com brilho, barra de acessibilidade e navegação
inferior em efeito vidro, orbes decorativos animados na tela de login, títulos
com leve gradiente de texto, e respeito a `prefers-reduced-motion`. Como quase
tudo no app usa as mesmas classes/variáveis CSS, essa camada eleva **todas as
telas e modais** sem precisar reescrever o HTML e sem risco de quebrar nenhuma
função existente.

## ⚠️ Ainda vale revisar
- Configure o `firebaseConfig` real em `firebase.js` (hoje está com placeholders)
  para o chat da comunidade funcionar.
- As credenciais do Google Sign-In (`gsi/client`) também precisam ser configuradas
  se você for usar login social de verdade.
