# TECNICOPROMAX — PROTOCOLO DE CONVERSAÇÃO CHATGPT ↔ OPENCODE VIA MCP

## 1. OBJETIVO

Este documento define o modo de colaboração permanente entre o ChatGPT e o OpenCode/Ben Code durante o desenvolvimento do Técnico Pro Max.

O objetivo é permitir uma troca real de mensagens entre os dois agentes quando a infraestrutura MCP/bridge disponível no YI Workstation permitir isso, evitando que o executor tome decisões críticas por conta própria quando houver dúvida, ambiguidade, conflito de fontes ou mudança estrutural.

Papéis:

- **ChatGPT:** comandante técnico, engenheiro, arquiteto e auditor.
- **OpenCode/Ben Code:** executor, pesquisador e implementador.
- **Usuário:** autoridade final para decisões de negócio, escopo e mudanças de alto impacto.

O OpenCode deve executar. O ChatGPT deve decidir arquitetura, metodologia, critérios de pesquisa, qualidade e tratamento de ambiguidades.

---

## 2. REGRA PRINCIPAL

Quando o OpenCode encontrar uma dúvida que possa alterar arquitetura, metodologia, estrutura de arquivos, interpretação técnica, confiabilidade de uma fonte, compatibilidade de placa, procedimento de bancada ou qualquer decisão difícil de reverter, ele NÃO deve adivinhar.

Ele deve consultar o ChatGPT pelo MCP/bridge disponível.

Se o MCP não estiver disponível, o OpenCode deve parar a etapa dependente daquela decisão e registrar a pendência. Não deve simular uma resposta do ChatGPT.

---

## 3. NÃO INVENTAR A INTERFACE MCP

Antes de implementar o modo de conversa, o executor deve auditar quais ferramentas MCP realmente estão expostas no YI Workstation.

Obrigatório:

1. listar os servidores MCP ativos;
2. listar as ferramentas disponíveis em cada servidor;
3. localizar a ferramenta real capaz de enviar uma mensagem para o ChatGPT e receber sua resposta;
4. documentar nome exato do servidor, ferramenta, parâmetros e formato da resposta;
5. testar uma mensagem simples;
6. somente depois implementar o protocolo.

NÃO inventar nomes de tools, endpoints, sockets, métodos ou APIs.

Se não houver uma ferramenta MCP que efetivamente converse com esta sessão/instância do ChatGPT, registrar isso como limitação e NÃO criar uma falsa integração.

---

## 4. MCP NÃO DEVE SER DESATIVADO DURANTE A EXECUÇÃO

Durante uma tarefa do Técnico Pro Max que dependa da colaboração ChatGPT ↔ OpenCode:

- não desativar o servidor MCP deliberadamente;
- não encerrar o bridge enquanto houver tarefa em andamento;
- não remover a configuração MCP para tentar corrigir outro problema;
- não substituir silenciosamente a comunicação MCP por uma resposta local simulada;
- não prosseguir como se tivesse recebido aprovação quando ocorreu timeout ou falha de conexão.

Se for tecnicamente necessário reiniciar o MCP, o executor deve primeiro salvar o estado da tarefa e, após o reinício, restaurar a conversa usando o mesmo identificador lógico de sessão quando a infraestrutura permitir.

---

## 5. TEMPO DE ESPERA DA RESPOSTA DO CHATGPT

Quando o OpenCode enviar uma consulta ao ChatGPT, deve aguardar **até 300 segundos** por resposta antes de considerar a tentativa como timeout.

Preferência de implementação:

1. usar uma chamada MCP bloqueante/assíncrona com `timeout = 300 s`, se a ferramenta suportar timeout;
2. se a ferramenta for assíncrona por ID de requisição, aguardar o resultado pelo mecanismo oficial do bridge;
3. somente se não existir espera nativa, usar polling moderado e limitado, sem busy-loop.

Regras:

- timeout máximo por tentativa: 300 segundos;
- não usar loop infinito;
- não consumir CPU em espera ativa;
- não cancelar a tarefa principal enquanto a consulta estiver aguardando, salvo erro fatal;
- manter o contexto da pergunta preservado durante a espera.

Ao atingir 300 segundos sem resposta:

- marcar `CHATGPT_TIMEOUT`;
- manter o MCP ativo;
- preservar a pergunta e o contexto;
- não assumir resposta;
- fazer no máximo uma nova tentativa automática quando a infraestrutura indicar que a conexão continua saudável;
- se ainda não houver resposta, suspender apenas a decisão dependente e apresentar a pendência ao usuário.

---

## 6. MODOS DE OPERAÇÃO

O executor deve trabalhar em três modos.

### 6.1 EXECUTE

Usado quando a tarefa já está claramente definida pela memória do projeto ou por instrução aprovada.

O OpenCode executa sem interromper o ChatGPT para decisões triviais.

Exemplos:

- criar uma pasta já especificada;
- atualizar um índice conforme template aprovado;
- traduzir material mantendo termos técnicos;
- organizar fontes;
- executar testes já definidos.

### 6.2 CONSULT

Usado quando existe uma dúvida pontual que exige decisão do ChatGPT.

Exemplos:

- duas fontes técnicas entram em conflito;
- não está claro se dois board IDs são realmente a mesma placa;
- uma revisão de PCB diverge do material encontrado;
- um novo tipo de arquivo pode exigir mudança estrutural;
- um valor técnico encontrado parece inconsistente.

### 6.3 DEBATE

Usado quando existem duas ou mais alternativas defensáveis e vale a pena os agentes discutirem antes da execução.

No modo DEBATE, o OpenCode deve apresentar:

- problema;
- evidências;
- alternativas;
- vantagens e riscos;
- sua recomendação;
- pergunta objetiva ao ChatGPT.

O ChatGPT pode responder com:

- `APPROVE`;
- `REJECT`;
- `REQUEST_EVIDENCE`;
- `COUNTERPROPOSAL`;
- `ASK_USER`;
- `EXECUTE_WITH_CONDITIONS`.

O debate continua até haver decisão clara ou necessidade de escalar ao usuário.

Não criar debate infinito. Como regra operacional, após aproximadamente 6 trocas sem convergência, resumir os pontos de divergência e solicitar decisão do usuário.

---

## 7. FORMATO PADRÃO DA CONSULTA DO OPENCODE

Toda consulta deve carregar contexto suficiente para o ChatGPT decidir sem precisar adivinhar.

Formato recomendado:

```text
TECNICOPROMAX_MCP_MESSAGE

SESSION_ID: <id estável da tarefa>
MESSAGE_ID: <id único>
MODE: CONSULT | DEBATE
TASK: <tarefa atual>
REPOSITORY: patricsouza22-gif/TECNICOPROMAX
BRANCH: <branch>
COMMIT_BASE: <sha quando aplicável>

QUESTION:
<pergunta objetiva>

CONTEXT:
<contexto mínimo necessário>

EVIDENCE:
- arquivo:linha ou URL
- arquivo:linha ou URL
- resultado de teste

OPTIONS:
A) ...
B) ...
C) ...

OPENCODE_RECOMMENDATION:
<recomendação do executor e justificativa>

RISK_IF_WRONG:
<impacto de uma decisão errada>

BLOCKING:
YES | NO

EXPECTED_RESPONSE:
APPROVE | REJECT | REQUEST_EVIDENCE | COUNTERPROPOSAL | ASK_USER | EXECUTE_WITH_CONDITIONS
```

Para uma dúvida simples, campos irrelevantes podem ser omitidos, mas nunca omitir o problema e as evidências essenciais.

---

## 8. FORMATO DA RESPOSTA DO CHATGPT

Quando possível, o bridge deve preservar a resposta textual integral do ChatGPT.

O OpenCode deve interpretar a decisão pelo conteúdo, não por heurística frágil de uma única palavra.

Formato lógico esperado:

```text
DECISION: <tipo>

REASONING_SUMMARY:
<justificativa objetiva e auditável>

INSTRUCTIONS:
1. ...
2. ...
3. ...

CONSTRAINTS:
- ...

NEED_USER:
YES | NO
```

A resposta deve ser registrada no log de colaboração quando ela alterar decisões relevantes do projeto.

---

## 9. CONTEXTO QUE O OPENCODE DEVE ENVIAR

O OpenCode não deve mandar ao ChatGPT apenas frases como "o que faço agora?".

Sempre que relevante, anexar/referenciar:

- arquivo e linhas relacionadas;
- board ID;
- revisão da placa;
- sintoma pesquisado;
- URLs das fontes;
- trecho traduzido e trecho original quando houver risco de tradução;
- logs de teste;
- erro exato;
- diff planejado;
- decisão anterior relacionada;
- estado atual da tarefa.

Quanto melhor o contexto, menor a chance de decisão errada.

---

## 10. ESTADO DA CONVERSA

Criar, quando a implementação começar, um estado de colaboração persistente, por exemplo em:

```text
.tecpro/
  collaboration/
    session.json
    decisions.jsonl
    pending.jsonl
```

Essa estrutura é conceitual e pode ser ajustada após auditoria do ambiente.

O estado deve permitir saber:

- sessão atual;
- tarefa atual;
- última pergunta enviada;
- última resposta recebida;
- perguntas pendentes;
- decisões aprovadas;
- timestamps;
- status do MCP;
- tentativas e timeouts.

Não salvar tokens, cookies, API keys, credenciais ou segredos nesses arquivos.

---

## 11. REGISTRO DE DECISÕES

Decisões importantes devem ser registradas de forma auditável.

Exemplo lógico:

```json
{
  "session_id": "TPM-20260912-001",
  "question": "Duas revisões usam o mesmo procedimento?",
  "decision": "Não misturar sem evidência",
  "decided_by": "ChatGPT",
  "timestamp": "...",
  "evidence": ["..."],
  "affected_files": ["..."]
}
```

Usar JSONL ou formato equivalente simples e legível.

---

## 12. QUANDO É OBRIGATÓRIO CONSULTAR O CHATGPT

Consulta obrigatória antes de prosseguir quando houver:

- mudança de arquitetura do repositório;
- alteração permanente da memória mestra;
- mudança na taxonomia principal;
- exclusão ou migração em massa de conteúdo;
- conflito técnico importante entre fontes;
- suspeita de que uma fonte esteja errada;
- inferência sobre board ID/revisão não comprovada;
- procedimento de bancada potencialmente destrutivo;
- valor de tensão/corrente/rail sem fonte confiável;
- dúvida sobre compatibilidade entre BIOS, EC, placa ou revisão;
- automação que possa sobrescrever dados existentes;
- decisão que afete muitas placas/modelos ao mesmo tempo.

---

## 13. QUANDO NÃO É NECESSÁRIO CONSULTAR

Não interromper o fluxo por tarefas mecânicas já aprovadas.

Exemplos:

- criar arquivo a partir de template vigente;
- corrigir ortografia sem alterar significado;
- atualizar link já confirmado;
- adicionar fonte ao final do arquivo;
- ordenar índice;
- executar teste previsto;
- converter texto de fonte para português mantendo conteúdo técnico.

O objetivo é colaboração, não burocracia.

---

## 14. TRATAMENTO DE FALHAS DO MCP

Estados recomendados:

- `MCP_READY`
- `MCP_WAITING_CHATGPT`
- `MCP_RESPONSE_RECEIVED`
- `MCP_TIMEOUT`
- `MCP_DISCONNECTED`
- `MCP_TOOL_ERROR`
- `MCP_RECOVERING`

Em `MCP_DISCONNECTED` ou `MCP_TOOL_ERROR`:

1. salvar estado local;
2. não perder a pergunta;
3. tentar recuperação controlada;
4. não desligar o restante da plataforma sem necessidade;
5. não inventar resposta;
6. se a decisão for bloqueante, parar essa linha de execução.

---

## 15. KEEPALIVE E SAÚDE

Se a infraestrutura MCP possuir mecanismo oficial de health check/keepalive, utilizá-lo.

Não criar tráfego excessivo apenas para "manter acordado".

Antes de uma consulta importante:

1. verificar conexão;
2. confirmar que a tool do ChatGPT continua registrada;
3. enviar a consulta;
4. aguardar até 300 segundos;
5. registrar o resultado.

---

## 16. NÃO FINGIR AUTONOMIA QUE NÃO EXISTE

O protocolo depende da infraestrutura real do YI Workstation.

O OpenCode não deve declarar que está conversando com o ChatGPT se estiver apenas:

- lendo um arquivo escrito anteriormente pelo ChatGPT;
- consultando uma resposta em cache;
- chamando outro modelo local;
- simulando a resposta;
- usando um prompt que imita o ChatGPT.

Deve haver uma troca real pelo bridge/MCP configurado pelo usuário.

---

## 17. RELAÇÃO COM A MEMÓRIA MESTRA

`MEMORIA_TECNICOPROMAX.md` continua sendo a especificação técnica principal do projeto.

Este arquivo define especificamente a colaboração entre agentes.

Em caso de conflito:

1. instrução explícita atual do usuário;
2. decisão explícita do ChatGPT para a tarefa;
3. `MEMORIA_TECNICOPROMAX.md`;
4. este protocolo;
5. convenções locais do executor.

Se ainda houver conflito, consultar o ChatGPT.

---

## 18. PROMPT OPERACIONAL PARA O OPENCODE/BEN CODE

```text
ATIVAR MODO DE COLABORAÇÃO TECNICOPROMAX.

Você é o EXECUTOR.
O ChatGPT é o COMANDANTE TÉCNICO / ENGENHEIRO / ARQUITETO.
O usuário é a AUTORIDADE FINAL.

Leia antes de trabalhar:
1. MEMORIA_TECNICOPROMAX.md
2. PROTOCOLO_CONVERSACAO_MCP.md

ANTES DE IMPLEMENTAR A CONVERSA:
- audite os servidores MCP realmente ativos;
- liste as tools MCP disponíveis;
- identifique qual tool REAL permite conversar com o ChatGPT conectado pelo usuário;
- não invente nome de tool ou endpoint;
- faça um teste simples de ida e volta.

REGRA PERMANENTE:
Quando houver dúvida crítica, conflito, mudança estrutural, risco técnico ou decisão difícil de reverter, consulte o ChatGPT pelo MCP antes de prosseguir.

TEMPO DE ESPERA:
Aguarde até 300 segundos por cada resposta do ChatGPT.
Se a tool suportar timeout, configure 300 s.
Se houver mecanismo assíncrono oficial, use-o.
Não faça busy-loop.
Não crie loop infinito.

MCP:
- não desative o MCP durante uma tarefa ativa;
- não encerre deliberadamente o bridge enquanto aguarda resposta;
- não substitua silenciosamente o ChatGPT por outro modelo;
- não simule resposta;
- se houver timeout, preserve o estado e a pergunta;
- não prossiga com decisão bloqueante sem resposta.

MODOS:
EXECUTE = tarefa clara e aprovada.
CONSULT = dúvida pontual.
DEBATE = alternativas técnicas/arquiteturais precisam ser discutidas.

NO MODO DEBATE:
Envie ao ChatGPT:
- problema;
- contexto;
- evidências;
- alternativas;
- riscos;
- sua recomendação.

Aceite como possíveis decisões:
APPROVE
REJECT
REQUEST_EVIDENCE
COUNTERPROPOSAL
ASK_USER
EXECUTE_WITH_CONDITIONS

Se após aproximadamente 6 trocas não houver convergência, resuma o impasse e peça decisão do usuário.

NÃO INICIE A BASE TÉCNICA AINDA.
Primeiro implemente/teste somente o canal de colaboração.

TESTE MÍNIMO OBRIGATÓRIO:
1. verificar MCP;
2. enviar ao ChatGPT:
   "TECNICOPROMAX MCP TESTE. Responda apenas: TECNICOPROMAX_MCP_OK";
3. aguardar até 300 s;
4. receber a resposta real;
5. registrar tempo de resposta;
6. confirmar que o MCP permaneceu ativo;
7. confirmar que a resposta veio da integração correta;
8. repetir uma segunda troca contextual para provar que existe continuidade.

SEGUNDO TESTE:
Pergunte ao ChatGPT:
"Estamos no projeto TECNICOPROMAX. Qual é o papel do OpenCode neste projeto?"

A resposta deve ser recebida pelo mesmo canal MCP e registrada apenas para validação.

AO TERMINAR, PARE E ENTREGUE:
- servidor MCP usado;
- tool MCP usada;
- parâmetros usados;
- mecanismo de timeout;
- como a espera de 300 s foi implementada;
- como o estado da conversa é preservado;
- resultado do primeiro teste;
- resultado do segundo teste;
- tempo de resposta;
- falhas encontradas;
- arquivos modificados;
- riscos restantes.

FINAL OBRIGATÓRIO:
STATUS_MCP_CHATGPT:
[FUNCIONANDO / PARCIAL / FALHOU]

WAIT_300S:
[PASS / FAIL]

MCP_PERMANECEU_ATIVO:
[PASS / FAIL]

TESTE_TECNICOPROMAX_MCP_OK:
[PASS / FAIL]

Não avance para pesquisa de placas antes da auditoria do ChatGPT.
```

---

## 19. ESTADO

Este protocolo foi definido como regra permanente do Técnico Pro Max em 2026-09-12.

A integração real ainda precisa ser auditada no ambiente local do YI Workstation antes de ser considerada funcional.