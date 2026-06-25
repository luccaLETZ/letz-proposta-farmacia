# CLAUDE.md — Proposta Farmácias (Let'z Gum)

Proposta comercial B2B **designada para farmácias** (pitch pra farmácia virar ponto de venda/revenda da Let'z Gum). Projeto **separado** da proposta B2B geral — repositório, link e timer próprios.

- **Dono / responsável:** Lucca (Comercial Let'z) — WhatsApp `5531975914447`
- **Repo:** github.com/luccaLETZ/letz-proposta-farmacia
- **Live (Vercel):** letz-proposta-farmacia.vercel.app
- **Arquivo principal:** `index.html` (página única — HTML + CSS + JS + imagens, tudo inline num arquivo só)
- **Status:** Produção

---

## Como o Lucca trabalha (LER SEMPRE)
- Lucca é **não-técnico** em git/CMD. Sempre dar **passo a passo** e **prompts prontos pra colar**, com **prints de conferência**, sem rodeios.
- **Falar português brasileiro.**
- **Workflow:** VS Code → `git push` → **Vercel** (deploy automático, branch `master`). Windows usando **CMD** (não PowerShell).
- **Regras de edição:**
  1. **Plano primeiro** → mostrar **ANTES/DEPOIS** → **esperar o Lucca confirmar** → só então **commit + push** direto na `master`.
  2. **Nunca abrir pull request.** Commit direto na `master`.
  3. Edições sempre **cirúrgicas** (mexer só no que foi pedido).
  4. Antes de aplicar, **conferir se o projeto aberto é `letz-proposta-farmacia`** (e não a proposta B2B `letz-proposta`).

---

## Bloco de configuração (CONFIG no `index.html`)
Fica no topo do `<script>`. Pra trocar de vendedor ou ajustar validade, mexer só aqui:

```javascript
const CONFIG = {
  whatsapp: "5531975914447",   // 55 + DDD + número, sem espaços. Todos os botões de WhatsApp usam isso.
  validadeHoras: 24,           // duração do timer da proposta
  storageKey: "letz_farmacia_v1" // chave do timer DESTE projeto. Não usar a mesma da proposta B2B.
};
```

---

## Comando "reiniciar timer"
Quando o Lucca disser **"reiniciar timer"**, reativar a contagem em **todos os dispositivos**.

A validade é salva **por aparelho** (localStorage), então a única forma de zerar pra todos é **trocar o `storageKey`**, subindo a versão (`letz_farmacia_v1` → `v2` → `v3`…), **sem repetir valor**. Não mexer nas horas. A nova contagem começa quando cada cliente **reabre o link** (não é retroativo).

---

## Avisos técnicos (lições da proposta B2B — conferir se valem aqui)
Estes dois pontos foram problemas reais que derrubaram conversão na proposta B2B (principalmente no **Samsung Internet**). O arquivo herdado do parceiro **ainda usa os padrões antigos** — avaliar com o Lucca se vale corrigir:

1. **Botões de toque no mobile:** o ideal é **UM** listener `click` num `<button>` de verdade, com `touch-action:manipulation` no CSS. O padrão herdado usa `touchstart`/`pointerdown`/`mousedown` + `preventDefault()` + `{ passive:false, capture:true }`, que no Samsung pode ser lido como rolagem e **cancelar o toque** (cliente precisa "segurar" pra abrir o plano).
2. **localStorage:** deve estar dentro de `try/catch`. No **modo Secreto do Samsung** o localStorage é bloqueado e, sem proteção, **trava a página**. A função `getExpiration()` herdada chama `localStorage` sem `try/catch`.

> Sempre **testar no Chrome anônimo do celular** ou pedir teste no **Samsung Internet** depois de qualquer mudança em botões/timer.

---

## Rastreamento
- A página usa eventos de comportamento via `trackClarity(...)` (Microsoft Clarity). Conferir se o script do Clarity está presente antes de `</head>` e qual o project id em uso.

---

## Deploy
- Push na `master` do repo `letz-proposta-farmacia` → a Vercel publica sozinha em `letz-proposta-farmacia.vercel.app`.
- Site **estático** (arquivo único): não precisa de build nem configuração extra na Vercel.
