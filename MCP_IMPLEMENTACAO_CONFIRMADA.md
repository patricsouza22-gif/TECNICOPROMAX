# TECNICOPROMAX — IMPLEMENTAÇÃO MCP CONFIRMADA

## 1. ESTADO

Em 2026-09-13 foi informado pelo executor que o YI Workstation já possui bridge/MCP ativo capaz de encaminhar mensagens do OpenCode ao ChatGPT.

A interface operacional reportada é:

```powershell
opencode mcp call chatgpt_ask --prompt "<mensagem>"
```

A tool MCP reportada é:

```text
chatgpt_ask
```

O bridge/MCP é descrito como independente do modo de terminal do OpenCode, portanto deve funcionar tanto no modo OpenCode normal quanto no modo FCC, desde que o terminal pertença à instância correta do YI Workstation e o bridge esteja ativo.

Este documento registra a interface reportada. Ela só deve ser considerada tecnicamente VALIDADA após um teste real de ida e volta bem-sucedido.

---

## 2. TESTE DE IDA E VOLTA

Com o YI Workstation em execução, enviar no terminal OpenCode integrado:

```powershell
opencode mcp call chatgpt_ask --prompt "TECNICOPROMAX MCP TESTE. Responda apenas: TECNICOPROMAX_MCP_OK"
```

Resposta esperada exatamente:

```text
TECNICOPROMAX_MCP_OK
```

Critério:

- resposta exata recebida pelo MCP: PASS;
- timeout, erro de tool ou resposta por canal diferente: FAIL/PARCIAL conforme o caso.

O mecanismo informado deve aguardar até 300 segundos pela resposta.

---

## 3. TESTE DE CONTINUIDADE

Após o primeiro teste, enviar pelo mesmo canal:

```powershell
opencode mcp call chatgpt_ask --prompt "Qual foi a pergunta que você acabou de responder?"
```

Objetivo: verificar se o bridge preserva o contexto da mesma conversa/sessão.

Não presumir continuidade apenas porque ambas as chamadas funcionaram. A segunda resposta precisa demonstrar conhecimento da interação anterior.

---

## 4. DIAGNÓSTICO DO MCP

Se a chamada falhar, executar:

```powershell
opencode mcp list
```

Verificar:

- se o servidor MCP esperado aparece;
- se `chatgpt_ask` está registrada;
- se o YI Workstation está realmente em execução;
- se o terminal utilizado pertence à instância integrada do YI Workstation e não a um OpenCode avulso.

Não desativar o MCP como tentativa genérica de correção.

---

## 5. USO NO TECNICOPROMAX

Depois de PASS no teste mínimo, o OpenCode deve usar `chatgpt_ask` para os modos CONSULT e DEBATE definidos em `PROTOCOLO_CONVERSACAO_MCP.md`.

Para a missão LA-B071P, o executor deve primeiro pesquisar e reunir evidências. Em seguida deve enviar ao ChatGPT uma mensagem estruturada de DEBATE contendo:

- missão atual;
- board ID e revisão;
- equipamentos associados;
- componentes e subsistemas identificados;
- sintomas documentados;
- causas alegadas;
- soluções alegadas;
- URLs/fontes;
- conflitos;
- itens sem evidência suficiente;
- recomendação do executor;
- perguntas objetivas ao ChatGPT.

O ChatGPT poderá responder com:

- `APPROVE`;
- `REJECT`;
- `REQUEST_EVIDENCE`;
- `COUNTERPROPOSAL`;
- `ASK_USER`;
- `EXECUTE_WITH_CONDITIONS`.

---

## 6. TRABALHO CONTÍNUO DURANTE A ESPERA

Enquanto uma decisão estiver aguardando resposta do ChatGPT, o executor pode continuar tarefas independentes e já autorizadas, como:

- pesquisa de fontes adicionais;
- tradução;
- catalogação de links;
- localização de datasheets;
- análise documental;
- organização de evidências.

Não executar a decisão que depende da resposta pendente.

---

## 7. REGRAS DE SEGURANÇA E RASTREABILIDADE

- Nunca simular resposta do ChatGPT.
- Nunca substituir `chatgpt_ask` por outro modelo sem informar.
- Nunca registrar tokens, chaves, cookies ou credenciais.
- Preservar pergunta, resposta, timestamp e decisão importante em log de colaboração.
- Manter timeout máximo de 300 segundos por tentativa conforme o protocolo vigente.
- Não criar busy-loop.
- Não desligar o MCP durante uma consulta ativa.

---

## 8. STATUS ATUAL

```text
MCP_TOOL_REPORTADA: chatgpt_ask
COMANDO_REPORTADO: opencode mcp call chatgpt_ask --prompt "..."
BRIDGE: reportado como ativo no YI Workstation
TIMEOUT: reportado como até 300 s
VALIDACAO_REAL_IDA_E_VOLTA: PENDENTE
VALIDACAO_CONTINUIDADE: PENDENTE
PROXIMA_MISSAO_APOS_PASS: MISSAO_001_LA-B071P
```
