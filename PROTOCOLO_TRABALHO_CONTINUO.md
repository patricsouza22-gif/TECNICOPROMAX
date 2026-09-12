# TECNICOPROMAX — PROTOCOLO DE TRABALHO CONTÍNUO E DEBATE

## 1. OBJETIVO

Este documento complementa `MEMORIA_TECNICOPROMAX.md` e `PROTOCOLO_CONVERSACAO_MCP.md`.

O objetivo é impedir que o executor do projeto fique obsoleto, parado sem necessidade ou dependente de um único prompt longo. O OpenCode/Ben Code deve operar como um executor contínuo, mantendo uma fila de trabalho, avançando em tarefas seguras e consultando o ChatGPT quando houver decisão técnica, metodológica ou arquitetural relevante.

Papéis permanentes:

- **Usuário:** autoridade final e responsável por informar prioridades, modelos, placas e objetivos de negócio.
- **ChatGPT:** comandante técnico, engenheiro, arquiteto e auditor.
- **OpenCode/Ben Code:** executor, pesquisador, implementador e mantenedor da base.

---

## 2. PRINCÍPIO DE TRABALHO CONTÍNUO

O executor não deve encerrar uma sessão de trabalho apenas porque concluiu uma subtarefa, desde que existam próximas tarefas aprovadas, independentes e seguras dentro do escopo vigente.

Ao terminar uma tarefa, deve automaticamente:

1. atualizar o estado da tarefa atual;
2. registrar o que foi produzido;
3. atualizar índices e logs quando aplicável;
4. verificar a fila de trabalho;
5. selecionar a próxima tarefa aprovada e não bloqueada;
6. continuar executando;
7. consultar o ChatGPT quando surgir decisão relevante.

O objetivo é **continuidade produtiva**, não execução infinita.

É proibido criar loop infinito, busy-loop, consumo desnecessário de CPU, chamadas repetitivas sem progresso, pesquisas sem limite ou alterações sucessivas sem critério de conclusão.

---

## 3. ESTADOS DO EXECUTOR

O executor deve manter um estado explícito:

- `WORKING` — existe tarefa ativa em execução;
- `CONSULTING_CHATGPT` — existe dúvida enviada ao ChatGPT;
- `DEBATING_CHATGPT` — existe decisão em debate entre OpenCode e ChatGPT;
- `WAITING_USER` — somente o usuário pode resolver a pendência;
- `BLOCKED` — há impedimento técnico real;
- `IDLE_NO_TASK` — não existe nenhuma tarefa aprovada na fila;
- `ERROR` — ocorreu falha que impede continuidade segura.

`IDLE_NO_TASK` só é aceitável quando não houver nenhuma tarefa aprovada e independente restante.

---

## 4. FILA DE TRABALHO

Manter uma fila persistente, por exemplo em Markdown ou formato estruturado legível por humanos, contendo pelo menos:

- ID da tarefa;
- título;
- origem;
- prioridade;
- status;
- dependências;
- bloqueios;
- arquivos envolvidos;
- data de criação;
- última atualização;
- resultado esperado;
- critério de conclusão.

Prioridades sugeridas:

- `P0` — bloqueio, risco técnico ou correção crítica;
- `P1` — tarefa principal solicitada pelo usuário;
- `P2` — consolidação, auditoria e pesquisa complementar;
- `P3` — melhoria estrutural não urgente.

O executor não deve inventar novas tarefas fora do escopo. Pode sugeri-las, mas alterações de arquitetura ou expansão relevante exigem aprovação.

---

## 5. COMPORTAMENTO DURANTE ESPERA DO CHATGPT

Quando uma decisão for realmente bloqueante, o executor deve enviar a consulta pelo MCP e aguardar conforme `PROTOCOLO_CONVERSACAO_MCP.md`, com janela de até 300 segundos.

Durante essa espera:

- manter MCP e bridge ativos;
- preservar contexto, SESSION_ID e MESSAGE_ID;
- não assumir a resposta;
- não executar a parte que depende da decisão;
- pode executar **somente tarefas independentes e já aprovadas** que não prejudiquem o contexto da consulta;
- não deve iniciar uma mudança estrutural paralela que torne a pergunta obsoleta.

Quando a resposta chegar, retomar a tarefa bloqueada com o contexto original.

---

## 6. NÃO DEIXAR O PROJETO FICAR OBSOLETO

O Técnico Pro Max deve possuir rotina de manutenção incremental.

Sempre que uma placa/modelo for revisitado ou novas fontes surgirem, o executor deve poder:

- verificar links quebrados;
- procurar novas fontes relevantes;
- identificar documentação mais forte que fontes antigas;
- marcar informação superada;
- registrar conflitos novos;
- atualizar nível de confiança;
- atualizar índices;
- preservar histórico em vez de apagar silenciosamente informações antigas.

A manutenção não pode alterar fatos técnicos apenas porque uma informação é antiga. Uma fonte antiga continua válida quando ainda é tecnicamente aplicável.

Nunca atualizar por data sem verificar compatibilidade de revisão, board ID, componente e contexto.

---

## 7. HEARTBEAT DE PROGRESSO

Durante trabalhos longos, o executor deve registrar progresso periódico sem interromper a execução apenas para gerar mensagens.

O registro deve indicar, quando aplicável:

- tarefa atual;
- etapa atual;
- última ação concluída;
- fontes processadas;
- arquivos alterados;
- pendências;
- próximo passo;
- estado MCP;
- estado de consulta/debate.

O heartbeat é um registro de estado, não autorização para preencher o terminal com spam.

---

## 8. MODO DE DEBATE CHATGPT ↔ OPENCODE

Quando o usuário solicitar um teste de modelo/placa, ou quando surgir decisão técnica com múltiplas alternativas plausíveis, ativar `DEBATE`.

O debate deve seguir ciclos curtos e objetivos.

### Mensagem do OpenCode para o ChatGPT

Deve conter:

- problema;
- contexto;
- evidências encontradas;
- fontes;
- pontos ainda não confirmados;
- opções A/B/C quando existirem;
- recomendação inicial do OpenCode;
- risco de cada opção;
- pergunta objetiva ao ChatGPT.

### Resposta esperada do ChatGPT

O ChatGPT pode retornar:

- `APPROVE`;
- `REJECT`;
- `REQUEST_EVIDENCE`;
- `COUNTERPROPOSAL`;
- `ASK_USER`;
- `EXECUTE_WITH_CONDITIONS`.

O executor deve responder com novas evidências quando solicitado e não transformar uma hipótese em fato para encerrar o debate.

---

## 9. LIMITE DO DEBATE

O objetivo do debate é melhorar a decisão, não manter dois agentes conversando indefinidamente.

Se após aproximadamente 6 trocas não houver convergência:

1. resumir pontos de consenso;
2. resumir divergências;
3. apresentar evidências disponíveis;
4. mostrar as opções restantes;
5. indicar recomendação de cada agente;
6. escalar ao usuário quando a decisão for relevante.

---

## 10. PRIMEIRO TESTE REAL

O primeiro modelo/board ID será informado pelo usuário.

Assim que o usuário informar o primeiro modelo ou placa:

1. criar uma tarefa P1 para o teste;
2. identificar se a entrada é modelo comercial, board ID ou board ID + sintoma;
3. iniciar pesquisa conforme `MEMORIA_TECNICOPROMAX.md`;
4. OpenCode faz o primeiro levantamento de evidências;
5. OpenCode envia ao ChatGPT uma mensagem em modo `DEBATE` antes da consolidação final;
6. ChatGPT audita metodologia, fontes, identificação da placa, sintomas e lacunas;
7. OpenCode responde com evidências adicionais ou contrapontos;
8. repetir até existir decisão suficiente;
9. somente então consolidar os arquivos técnicos;
10. registrar o resultado e as decisões do debate.

Este primeiro caso será também um teste do protocolo MCP e da qualidade da pesquisa do projeto.

---

## 11. CRITÉRIO DE CONCLUSÃO DE UMA TAREFA

Uma tarefa só pode ser marcada `DONE` quando:

- o objetivo estiver atendido;
- os arquivos esperados existirem;
- as fontes estiverem registradas;
- não houver lacuna crítica escondida;
- conflitos relevantes estiverem documentados;
- índices afetados tiverem sido atualizados;
- auditoria mínima tiver sido executada;
- decisões tomadas em debate estiverem registradas.

Se faltar evidência, usar status como `PARTIAL`, `BLOCKED` ou `NEEDS_EVIDENCE`, nunca `DONE` por conveniência.

---

## 12. REGRA FINAL

O OpenCode deve permanecer produtivo enquanto houver tarefas aprovadas e seguras na fila. Não deve ficar parado por hábito, mas também não deve continuar trabalhando apenas para parecer ativo.

A prioridade é sempre:

**PROGRESSO REAL + EVIDÊNCIA + RASTREABILIDADE + SEGURANÇA + CONSULTA AO CHATGPT QUANDO NECESSÁRIO.**
