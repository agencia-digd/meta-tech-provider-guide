# Como virar Meta Tech Provider e conseguir Advanced Access

> Guia prático de quem passou pelo processo inteiro — do zero até **15 permissões
> em Acesso Avançado** num app que opera WhatsApp Business, Instagram e Ads em
> nome de **múltiplos clientes** (SaaS multi-tenant).
>
> Escrito em português, sem enrolação, com os **gotchas que custam dias** se
> ninguém te avisar.

🎥 **Vídeo acompanhando este guia:** https://youtu.be/fRy7fpq0NzA

---

## Pra quem é isso

Você está construindo um produto que precisa, **em nome de outras empresas**:

- enviar/ler mensagens de WhatsApp Business (Cloud API)
- publicar / ler comentários / responder DM no Instagram
- criar e gerenciar campanhas de Ads e puxar leads de Lead Ads

Enquanto você só mexe nos **seus próprios** ativos (sua Página, seu Instagram,
sua conta de anúncio), o **Standard Access** já basta e tudo funciona em modo
dev. No momento em que você quer operar o ativo **de um cliente**, a Meta exige
que você seja um **Provedor de Tecnologia (Tech Provider)** com **Advanced
Access** nas permissões. É esse pulo que este guia cobre.

---

## O mapa mental (leia antes de começar)

São **três portões independentes**. Gente trava porque confunde os três:

| Portão | O que é | Libera o quê |
|---|---|---|
| **1. App Review** | Análise das permissões do seu app, com screencast demonstrando cada uma | Standard → Advanced em cada permissão |
| **2. Business Verification** | Verificação da sua empresa (documento, endereço) | Pré-requisito do portão 3 |
| **3. Access Verification** (Data Protection / Tech Provider) | Questionário de proteção de dados + atestar que você é provedor de tecnologia | "Destrava" o Advanced pra operar ativos de **terceiros** |

⚠️ **App Review aprovado NÃO significa Advanced ativo.** Quando você submete, as
permissões entram em fila como **"Standard — Análise do app pendente"**. Só
viram **Advanced** depois que os **três** portões fecham. Muita gente acha que
"foi aprovado" e na real só entrou na fila.

---

## Passo a passo

### 0. Estrutura mínima

- Um **App** do tipo Business no [developers.facebook.com](https://developers.facebook.com).
- Um **Business Manager (BM)** com o app vinculado.
- Um **System User** na BM, com token que carregue **todos os escopos** que você
  vai submeter. Esse token **não expira** e é o que você usa na demo.

> 🔑 **Por que System User:** o screencast do App Review precisa mostrar o token
> **do próprio app** fazendo cada chamada. Se você demonstrar usando uma
> integração de terceiro (Kommo, uChat, etc.), **não conta** — eles usam o app
> deles, não o seu.

### 1. Verificações primeiro (portões 2 e 3)

No painel da BM → **Configurações → Central de Segurança**:

1. **Business Verification** — sobe documento da empresa. Pode levar dias.
2. **Verifique também o domínio** do seu produto.
3. **Access Verification / Data Protection Assessment** — questionário sobre como
   você trata dados de usuário. É o que normalmente segura o status de Tech
   Provider. Responda com calma.

> 🪤 **Gotcha:** verificação feita na **BM errada** não vale. Se você tem várias
> BMs (principal + backup), garanta que empresa **e** domínio estão verificados
> na **mesma BM que é dona do app**. Domínio verificado numa BM "backup" não
> destrava o app da BM principal.

### 2. Prepare a demonstração (portão 1)

A Meta quer um **screencast** mostrando cada permissão sendo usada de verdade,
**com o token do seu app**, numa conta real (pode ser a sua própria, ou a de um
cliente piloto que te deu acesso aos ativos).

A estratégia que funciona: **um app só, uma submissão consolidada**, com todas as
permissões demonstradas na mesma conta real. Pra cada permissão, um scriptzinho
que faz a chamada e imprime a resposta limpa na tela.

**Dica de ouro:** faça um **"sanitizer"** no output dos seus scripts de demo que
**remove `access_token`, paging e qualquer dado sensível** antes de imprimir — o
vídeo vai pra Meta, não dá pra vazar token na gravação.

#### Roteiro de demo por família de permissão

- **WhatsApp** (`whatsapp_business_messaging`, `whatsapp_business_management`):
  registrar o número, enviar um template, mostrar a entrega real.
- **Instagram** (`instagram_content_publish`, `instagram_manage_comments`,
  `instagram_manage_messages`, `instagram_manage_contents`,
  `instagram_manage_insights`, `instagram_basic`): publicar um post, comentar e
  responder, ler/responder DM, puxar insight (reach), apagar conteúdo.
- **Ads** (`ads_management`, `ads_read`): listar contas, **criar → editar (ativar)
  → pausar** uma campanha (sem ad set ela não gasta nada), e mostrar relatório.
- **Leads** (`leads_retrieval`): puxar o lead mais recente de um formulário de
  Lead Ads (use o **Lead Ads Testing Tool** pra gerar lead de teste sem PII real).
- **Pages** (`pages_show_list`, `pages_read_engagement`, `pages_messaging`,
  `pages_manage_metadata`): listar páginas, ler engajamento, etc.

### 3. Submeta e aguarde

Submetido = fila. O ciclo costuma levar **~5 dias úteis**. Enquanto isso, as
permissões aparecem como **Standard / Análise pendente**, mas seus demos **já
rodam** porque Standard Access cobre os **seus próprios ativos**.

### 4. Resultado

Quando fecha, no painel **Permissões e Recursos** cada permissão operacional
aparece como **"Acesso avançado — Acesso avançado concedido"**. É isso que
libera operar ativos de **outras empresas**. No nosso caso fecharam **15**:

```
pages_manage_metadata      instagram_manage_comments    ads_management
pages_messaging            instagram_manage_messages    ads_read
pages_show_list            instagram_manage_contents    leads_retrieval
pages_read_engagement      instagram_content_publish    whatsapp_business_messaging
instagram_basic            instagram_manage_insights    whatsapp_business_management
```

---

## Os gotchas que custaram tempo (a parte que vale ouro)

### WhatsApp: erro `131037` "display name needs approval"
Trava **todo** envio de template, independente de qualquer outra config. Se você
está num **número de teste** cujo nome de exibição foi **DECLINED**, **não há
conserto** — número de teste não deixa reeditar o display name (nem painel nem
API). **Solução:** use o número de teste **auto-provisionado** do app (aba
"Configuração da API" / "API Setup"), cujo `verified_name = Test Number` e
`name_status = APPROVED`. Pegue o **Phone Number ID** dali.

### WhatsApp: `133010` "Account not registered"
Antes do **primeiro** envio, registre o número:
```
POST /{phone_number_id}/register
{ "messaging_product": "whatsapp", "pin": "123456" }
```

### WhatsApp: `131030` destinatário não permitido
Número de teste só entrega pra destinatários na **allow-list**. Adicione o número
no painel → confirme o código → check verde.

### Instagram DM: precisa de Tester
`instagram_manage_messages` em modo dev só troca mensagem com quem tem **papel no
app** (Instagram Tester). Com qualquer outra conta dá **Timeout / erro #-2 /
2534084**. Adicione a conta-alvo como Instagram Tester e aceite o convite. (É
justamente isso que o **Advanced Access** remove na produção.)

### Token errado pra DM e Lead Forms
Endpoints de **DM do Instagram** e de **leadgen_forms** exigem **Page Token**, não
o token de System User. Resolva via `GET /me/accounts` e use o token da página.

### Timeout intermitente na API de conversas
A chamada de `conversations` do IG às vezes dá **TimeoutError**. Coloque um
**timeout de ~60s com 1 retry** nos seus scripts de demo, senão a gravação mostra
um traceback feio na hora errada.

### App Review ≠ Advanced
(repetindo porque é o erro nº1) submeter coloca na fila em **Standard**. Advanced
só vem depois dos três portões. Não comemore na submissão.

---

## Permissões que podem ser **rejeitadas** sem te atrapalhar

Algumas permissões **legadas ou redundantes** podem voltar como **"Análise do app
rejeitada"** — e **tudo bem**, porque:

1. a rejeição é só pro **Advanced**; elas continuam **ativas em Standard** (seus
   próprios ativos);
2. provavelmente existe uma permissão **moderna** que cobre o caso de uso.

Exemplos clássicos:

- **`read_insights`** — Page Insights legada (deprecada). Coberta por
  `instagram_manage_insights` + `pages_read_engagement`. **Não vale ressubmeter.**
- **`pages_read_user_content`** — ler conteúdo de **terceiros** no mural de uma
  Página do Facebook. Se você só lê comentário de **Instagram**, use
  `instagram_manage_comments` e ignore esta.

👉 Antes de brigar por uma permissão rejeitada, pergunte: **"meu produto
realmente usa isso, ou tem uma permissão moderna que já cobre?"**

---

## Checklist final

- [ ] App Business criado + vinculado à BM dona
- [ ] System User com token (não-expira) carregando todos os escopos
- [ ] Business Verification ✅ na **BM correta**
- [ ] Domínio verificado na **mesma BM**
- [ ] Access Verification / Data Protection respondido
- [ ] Scripts de demo com **sanitizer** de token no output
- [ ] Número de teste WhatsApp com `name_status = APPROVED` + registrado + allow-list
- [ ] Conta IG-alvo adicionada como **Instagram Tester**
- [ ] Screencast cobrindo **cada** permissão com o token **do app**
- [ ] Submetido → aguardar ~5 dias úteis
- [ ] Conferir painel: permissões em **"Acesso avançado concedido"**

---

## Licença

Compartilhe à vontade. Feito pra economizar dias de cabeça de quem vem depois.

🎥 https://youtu.be/fRy7fpq0NzA
