# Como virar Meta Tech Provider e conseguir Advanced Access

> Guia em PT-BR para quem vai operar ativos de outras empresas com WhatsApp
> Cloud API, Instagram e Ads. Original de 25/06/2026; revisão documental em
> **24/09/2026**. Relatos não confirmados estão identificados no fim.
>
> A aprovação depende do caso de uso e da análise da Meta. O objetivo aqui é
> preparar uma submissão que outra pessoa consiga reproduzir e avaliar.

🎥 [Vídeo do relato original](https://youtu.be/fRy7fpq0NzA)

## Entenda o que precisa ser aprovado

Standard Access não significa acesso irrestrito a qualquer ativo próprio:
existem requisitos de papel no app, autorização e acesso ao ativo. Para atender
outras empresas, peça Advanced Access nas permissões necessárias ao produto.
Testes com administradores/desenvolvedores não comprovam acesso de clientes.
Fontes: [níveis de acesso][access-levels] e [App Review][app-review].

| Processo | O que comprova | Como conferir |
|---|---|---|
| **Business Verification** | Identidade da empresa vinculada ao app | Empresa verificada no portfólio empresarial (antigo BM) correto |
| **App Review / Advanced Access** | Uso permitido de cada permissão ou recurso | Aprovação e nível de acesso de cada item no painel |
| **Access Verification** | Que a empresa atua como Tech Provider ao acessar dados de outras empresas | Resultado na seção de verificações do app |
| **Data Protection Assessment (DPA)** | Práticas de tratamento e proteção de dados, quando exigido | Avisos e avaliação próprios no painel |

**Access Verification é independente de App Review e dos níveis de acesso.**
A lista oficial inclui `whatsapp_business_management` e várias permissões de
Ads, Pages e Instagram. Não é correto dizer que três aprovações sempre mudam
juntas o rótulo Standard para Advanced. DPA também não é sinônimo de Access
Verification. [Access Verification][access-verification] · [DPA][dpa].

## 1. Prepare o app e a empresa

1. Crie um app com o **caso de uso WhatsApp** e vincule o portfólio da empresa
   que prestará o serviço. No painel atual: **Use cases → Customize → WhatsApp
   → Tech Provider onboarding**. Os nomes podem variar com o idioma e o app.
2. Faça **Business Verification antes de submeter o App Review de WhatsApp**.
   Um administrador da empresa confirma os dados cadastrais e o vínculo com a
   empresa; documentos podem ser solicitados. Confira se o app está conectado
   à empresa efetivamente verificada, não a outro portfólio seu.
3. Preencha as configurações básicas: ícone, categoria, finalidade para
   clientes, contato monitorado, política de privacidade, termos e instruções
   ou mecanismo de exclusão de dados. Os links precisam funcionar.
4. Quando solicitado, complete **Access Verification** em **Settings / Basics
   → Verifications → Access verification**. Descreva o serviço e o uso dos
   dados de outras empresas. A empresa precisa estar verificada e sem
   restrições. Responda às perguntas de tratamento de dados e ao DPA quando
   aparecerem; descreva o que o produto realmente faz.

Fontes: [onboarding de Tech Provider][tech-provider], [Business Verification][business-verification],
[configurações básicas][basic-settings] e [Access Verification][access-verification].

## 2. Faça o produto funcionar antes de gravar

Construa um fluxo reproduzível, com ambiente acessível ao revisor e instruções
passo a passo. Use dados fictícios e recursos de teste nas demonstrações.
Não coloque dados de clientes, credenciais pessoais, tokens ou identificadores
reais neste repositório ou em material público.

- Para WhatsApp, comece pelos recursos de teste da Cloud API e pelo painel
  **API Setup**. O sandbox de Embedded Signup é diferente: ele testa o cadastro,
  mas seu número não envia nem recebe mensagens.
- Faça **ao menos uma chamada bem-sucedida por permissão nos 30 dias anteriores
  à submissão**, conforme o tutorial geral de App Review.
- Escolha o token conforme o fluxo. Para ativos próprios de WhatsApp, a Meta
  orienta System User; para clientes de Tech Provider, **Business Integration
  System User (BISU)**, obtido pelo Embedded Signup. O token temporário do painel
  serve para começar os testes. A geração de System User permite escolher a
  expiração: não assuma que todo token “não expira”.
- Além dos escopos, confira o acesso do usuário ao ativo. Token com permissão
  não substitui a concessão de acesso à WABA.
- Oculte tokens, cabeçalhos de autorização, segredos, PINs e dados pessoais
  durante a gravação. Remover apenas `access_token` do JSON não basta; revise
  também terminal, URLs, paginação, notificações e logs.

Fontes: [tutorial de submissão][submission], [Cloud API][cloud],
[tokens][tokens] e [Embedded Signup][embedded].

## 3. Monte o pacote de App Review

Peça **somente permissões usadas pelo produto**. Você pode enviar permissões
juntas ou separadamente; não precisa reunir WhatsApp, Instagram e Ads para
começar. Para WhatsApp, não é necessário esperar o Embedded Signup inteiro
ficar pronto para iniciar a análise. [App Review de WhatsApp][wa-review].

Para cada permissão, prepare:

- **Descrição própria:** funcionalidade, benefício para o cliente, dados
  acessados e por que essa permissão é necessária.
- **Vídeo próprio:** início do fluxo, autorização quando aplicável, ação no
  produto e resultado visível. O guia de WhatsApp exige clipes separados por
  permissão; um vídeo misturando casos de uso pode ser rejeitado.
- **Instruções para teste:** URL, sequência de telas, dados fictícios e forma
  de acesso ao ambiente. Se houver login próprio, explique como o revisor
  entra. Não forneça credenciais da sua conta pessoal da Meta.

Prefira interface em inglês quando possível; caso contrário, inclua legendas
explicativas. Não dependa de áudio: o tutorial informa que os revisores não o
escutam. Um script mostrando JSON não substitui automaticamente a demonstração
do fluxo do produto. [Tutorial geral][submission].

### WhatsApp: as duas demonstrações essenciais

| Permissão | O que demonstrar |
|---|---|
| `whatsapp_business_messaging` | Criar/enviar uma mensagem e mostrar seu recebimento no WhatsApp web ou móvel |
| `whatsapp_business_management` | Criar um template pelo app ou pelo WhatsApp Manager |

A Meta permite demonstrar o envio com o cURL do **API Setup** e a criação de
modelo pelo **WhatsApp Manager**. No fluxo com Solution Partner, a documentação
também aceita demonstração da integração com o parceiro. Portanto, a proibição
genérica de “integração de terceiro não conta” estava incorreta. Essas alternativas
são documentadas para WhatsApp; não as generalize para outros produtos.
Fontes: [Tech Provider][tech-provider] e [App Review de WhatsApp][wa-review].

### Instagram, Ads, Leads e Pages

Use o mesmo princípio: cada permissão precisa de uma ação observável, e a
referência do endpoint define os escopos e tokens. Exemplos de roteiro:
publicar/responder comentário no Instagram; demonstrar gestão e relatório em
Ads; recuperar um lead de teste; listar páginas ou consultar seus insights.
Mantenha campanhas de demonstração pausadas e dados de leads fictícios.

**Escolha primeiro o login do Instagram.** O fluxo com Instagram Login usa
permissões como `instagram_business_basic` e
`instagram_business_manage_messages`, além de Instagram User access token.
Não copie cegamente os escopos ou a regra “DM sempre exige Page Token” do fluxo
antigo. [Mensagens com Instagram Login][ig-messages].

**`read_insights` não deve ser descartada como deprecada.** A referência atual de
Page Insights ainda exige `read_insights` e `pages_read_engagement`, com Page
access token e tarefa `ANALYZE`. Insights do Instagram não substituem os de
Página. [Referência de Insights][insights]. Para Leads, confira também o acesso
no Leads Access Manager e os requisitos da operação desejada: a documentação
aceita Page ou User access token em cenários de recuperação de leads.
[Recuperação de leads][leads].

## 4. Submeta, acompanhe e corrija a causa da rejeição

Clique em **Submit for Review**: um rascunho não entra na fila. Confira o status
individual das permissões; submissão recebida não é aprovação.

Motivos de rejeição documentados pela Meta:

- Permissão desnecessária ou uso fora da finalidade permitida.
- App inacessível, instruções insuficientes ou funcionalidade impossível de testar.
- Falta de descrição ou vídeo para determinada permissão.
- Screenshot no lugar do vídeo; no WhatsApp, vídeo misturando permissões/casos de uso.

Ao receber uma rejeição, leia o motivo daquele item, reproduza o teste como um
usuário novo, corrija o bloqueio e reenvie a evidência correspondente. Se o
produto realmente precisa da permissão, a rejeição impede esse uso em clientes;
não suponha que Standard ou outra permissão “moderna” resolva.
Fontes: [App Review][app-review], [tutorial][submission] e [WhatsApp][wa-review].

### Prazo: separe as etapas

| Etapa | O que a documentação consultada informa |
|---|---|
| Business Verification | Não foi confirmado prazo numérico nesta revisão; acompanhe o painel e eventuais pedidos de documentos |
| App Review de WhatsApp | Média de aproximadamente **24 horas** na página específica |
| App Review geral | Tutorial orienta esperar uma decisão **dentro de uma semana** |
| Access Verification | Decisão em aproximadamente **5 dias**, sem dizer “úteis” |

São referências de etapas diferentes, **não um SLA nem prazo total garantido**.
O relato original de “~5 dias úteis” não serve como promessa para o processo
inteiro. Reserve margem para documentos, correções e nova análise; não foi
confirmada uma média real de ponta a ponta. Fontes: [WhatsApp][wa-review],
[tutorial geral][submission] e [Access Verification][access-verification].

## 5. Termine o onboarding e valide com um cliente

Advanced Access não faz o cadastro do cliente sozinho. Depois das aprovações:

1. Confira as permissões concedidas, as verificações e as pendências do painel.
   Passe para **Live**, quando aplicável, depois do App Review.
2. Implemente **Embedded Signup com Facebook Login for Business**. Use o fluxo
   atual **v4**: a documentação anuncia o fim do v2 em **15/10/2026**. Siga a
   configuração oficial; a versão do Embedded Signup é distinta da Graph API.
3. No servidor, troque o código retornado pelo fluxo pelo token BISU do cliente.
   Guarde o vínculo entre cliente, WABA e número; mantenha tokens fora do frontend.
4. Configure o webhook do app e assine a WABA do cliente com
   `POST /<WABA_ID>/subscribed_apps`. No fluxo padrão, registre o número
   verificado com `POST /<PHONE_NUMBER_ID>/register` e o PIN de verificação em
   duas etapas. Para número já usado no WhatsApp Business app, siga o fluxo
   específico de coexistência indicado na documentação de Tech Provider.
5. Oriente o cliente a adicionar a forma de pagamento à WABA. No modelo de
   Tech Provider, isso faz parte da conclusão do onboarding.
6. Como validação prática, teste com um usuário autorizado **sem papel no app**:
   onboarding, envio, recebimento e eventos de entrega. Não trate apenas o
   sucesso da requisição de envio como confirmação de entrega.

Fontes: [Embedded Signup][embedded], [versões][es-versions],
[onboarding de clientes][onboarding], [registro][registration] e [Tech Provider][tech-provider].
Acompanhe depois os avisos de DPA e Data Use Checkup; aprovação inicial não
encerra as obrigações de manutenção de acesso. [DPA][dpa] · [Níveis de acesso][access-levels].

## Preços e categorias de template

Desde **01/07/2025**, a cobrança de mensagens da Meta é por **template entregue**,
considerando categoria e mercado do destinatário. Em 24/09/2026, a página oficial
identifica a tabela vigente como **01/07/2026** e anuncia mudanças para
**01/10/2026**. Consulte a tabela aplicável à moeda da WABA antes de orçar;
este guia não fixa valores por mensagem. [Preços oficiais][pricing].

- A mensagem do usuário abre ou renova a janela de atendimento de **24 horas**.
  Mensagens livres só podem ser enviadas nessa janela e são gratuitas pela Meta.
- Templates **utility** entregues dentro da janela também são gratuitos.
  Fora dela, são cobrados. Marketing e authentication são cobrados, salvo
  exceções como a janela gratuita de entrada elegível.
- Há descontos por volume para utility/authentication. A janela gratuita de
  **72 horas** exige entrada por anúncio Click-to-WhatsApp ou botão de Página
  no WhatsApp móvel e resposta da empresa em até 24 horas. Começa nessa
  resposta; se a janela de atendimento fechar, só templates podem ser enviados.

| Categoria | Como classificar |
|---|---|
| **Marketing** | Promoção, oferta, retomada comercial; conteúdo misto de serviço e promoção também entra aqui |
| **Utility** | Sem promoção, ligado a pedido/ação específica do usuário ou informação essencial/crítica conforme os critérios da Meta |
| **Authentication** | Verificação de identidade com código de uso único, usando os formatos próprios de autenticação |

A Meta pode aprovar um template solicitado como utility na categoria marketing
e reclassificar templates depois. Monitore categoria e status; a cobrança segue
a categoria aplicada no uso. Aprovar um template é um processo diferente de
aprovar o app. [Categorização oficial][categories].

Essas são as regras gerais. A Meta publica uma [política específica para
AI Providers][ai-pricing], que pode cobrar mensagens livres em mercados
aplicáveis; não estenda a gratuidade geral a esse caso.

**Brasil:** desde **01/07/2026**, clientes elegíveis com país *Sold-To* Brasil no
Billing Hub só podem criar novas WABAs em BRL. A Meta orienta migrar as WABAs
existentes desses clientes até **30/06/2027**, com interrupção de entrega nas
não migradas a partir de **01/07/2027**. Confira a elegibilidade na seção
*Billing localization launches*; não é uma regra baseada no país do destinatário.
[Preços e localização de cobrança][pricing].

## Erros e relatos do guia original

**WhatsApp `131037`:** a referência atual relaciona o erro a número empresarial
**555** sem nome de exibição aprovado e orienta alterar o nome. A afirmação antiga
“não há conserto” foi corrigida. Não confunda esse número com o recurso de teste
autoprovisionado do app. [Erros][errors] · [Embedded Signup][embedded].

**WhatsApp `133010`:** número não registrado. Confira a adição/verificação do
número e faça o registro pela API; o PIN é o de verificação em duas etapas, não
um valor fixo para copiar do guia. [Erros][errors] · [Registro][registration].

Os pontos abaixo ficam preservados como **relatos não confirmados em 24/09/2026**,
sem transformá-los em requisitos universais. Detalhes em [REVISAO-ASTRA.md](REVISAO-ASTRA.md).

- Domínio: o original orienta “verifique também o domínio” na mesma BM dona do
  app e diz que verificar na BM de backup não destrava o app principal.
- WhatsApp `131030`: o original orienta adicionar o destinatário de teste à
  allow-list no painel, confirmar o código e conferir o check verde.
- Instagram DM em modo dev: o original associa Timeout / `#-2` / `2534084` à
  ausência de Instagram Tester, orientando adicionar a conta e aceitar o convite.
- `pages_read_user_content`: o original a descreve como leitura de conteúdo
  de terceiros no mural da Página e recomenda não solicitá-la para comentários
  exclusivamente de Instagram; a referência específica não ficou acessível.
- `leadgen_forms`: o original orienta Page Token obtido por `GET /me/accounts`,
  em vez de System User. Não foi confirmada aqui a exigência exclusiva desse
  token para esse endpoint.
- Timeout de `conversations` do Instagram: o original sugere ~60 segundos com
  uma repetição nos scripts de demo; é ajuste empírico, não prazo oficial.
- O relato de **15 permissões aprovadas** não foi auditado em um app real. A
  lista histórica era: `pages_manage_metadata`, `pages_messaging`,
  `pages_show_list`, `pages_read_engagement`, `instagram_basic`,
  `instagram_manage_comments`, `instagram_manage_messages`,
  `instagram_manage_contents`, `instagram_content_publish`,
  `instagram_manage_insights`, `ads_management`, `ads_read`, `leads_retrieval`,
  `whatsapp_business_messaging` e `whatsapp_business_management`. Não use como
  lista obrigatória; a disponibilidade de `instagram_manage_contents` não foi
  confirmada nesta revisão.

## Checklist final

- [ ] App vinculado à empresa correta e Business Verification concluída.
- [ ] Configurações, privacidade, termos e exclusão de dados acessíveis.
- [ ] Permissões limitadas ao produto; chamadas de teste recentes bem-sucedidas.
- [ ] Vídeo e justificativa por permissão; ambiente reproduzível pelo revisor.
- [ ] Gravações e logs sem segredos ou dados de clientes.
- [ ] Submissão enviada, não apenas salva como rascunho.
- [ ] Advanced Access e Access Verification conferidos separadamente; demais avaliações atendidas quando exigidas.
- [ ] Embedded Signup atual, token por cliente, registro, webhooks e pagamento configurados.
- [ ] Fluxo validado com usuário autorizado sem papel no app.
- [ ] Categorias e tabela de preços vigentes conferidas antes de oferecer o serviço.

## Compartilhamento

Compartilhe à vontade. Feito para economizar tempo de quem vem depois.
O vídeo registra a experiência original; as orientações revisadas e suas fontes
estão neste texto e em [REVISAO-ASTRA.md](REVISAO-ASTRA.md).

[tech-provider]: https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/get-started-for-tech-providers
[app-review]: https://developers.facebook.com/documentation/resp-plat-initiatives/individual-processes/app-review
[wa-review]: https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/app-review
[business-verification]: https://developers.facebook.com/documentation/development/release/business-verification
[access-verification]: https://developers.facebook.com/documentation/development/release/access-verification
[access-levels]: https://developers.facebook.com/docs/graph-api/overview/access-levels/
[dpa]: https://developers.facebook.com/documentation/resp-plat-initiatives/individual-processes/data-protection-assessment
[basic-settings]: https://developers.facebook.com/documentation/development/create-an-app/app-dashboard/basic-settings
[submission]: https://developers.facebook.com/documentation/resp-plat-initiatives/individual-processes/app-review/submission-guide
[cloud]: https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform
[tokens]: https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens
[embedded]: https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview
[es-versions]: https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions
[onboarding]: https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-tech-provider
[registration]: https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/registration
[pricing]: https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing
[categories]: https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization
[errors]: https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes
[ig-messages]: https://developers.facebook.com/documentation/instagram-platform/instagram-api-with-instagram-login/messaging-api
[insights]: https://developers.facebook.com/docs/graph-api/reference/insights/
[leads]: https://developers.facebook.com/documentation/ads-commerce/marketing-api/guides/lead-ads/retrieving
[ai-pricing]: https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/ai-providers
