# ADENDO 001 — MCP NÃO BLOQUEIA PESQUISA DA LA-B071P

## Regra operacional imediata

Este adendo esclarece a MISSAO_001_LA-B071P e os protocolos de colaboração.

A indisponibilidade, não validação ou timeout do MCP NÃO autoriza o OpenCode/Ben Code a ficar ocioso e NÃO bloqueia a pesquisa independente da placa LA-B071P.

O MCP é obrigatório apenas para decisões que dependam de aprovação, debate ou auditoria do ChatGPT, como:

- mudança de arquitetura;
- consolidação de conflito entre fontes;
- extrapolação entre revisões/placas;
- aprovação de valor elétrico suspeito;
- procedimento potencialmente destrutivo;
- migração estrutural grande;
- conclusão técnica que dependa de julgamento do ChatGPT.

Enquanto o MCP não estiver validado ou estiver indisponível, o executor DEVE continuar normalmente com trabalho seguro e independente, incluindo:

- pesquisa web e multilíngue;
- identificação da placa e notebooks associados;
- coleta e catalogação de fontes;
- localização de esquemas, boardviews, manuais e datasheets;
- levantamento de revisões e variantes;
- inventário de componentes;
- pesquisa de sintomas;
- coleta de casos reais;
- tradução para português;
- registro de URLs e evidências;
- separação entre casos documentados e falhas apenas possíveis;
- preenchimento de arquivos factuais provisórios;
- criação de PESQUISA_LA-B071P_STATUS.md;
- preparação de DEBATE_CHATGPT_LA-B071P.md;
- commits lógicos do material pesquisado.

O executor deve marcar como PENDENTE apenas aquilo que realmente precisa de decisão do ChatGPT.

## Regra anti-ociosidade

Se houver qualquer frente segura e independente disponível, o estado correto é `WORKING`, não `WAITING_MCP`.

`WAITING_MCP` só pode bloquear a subdecisão dependente da consulta, nunca toda a missão.

Exemplo:

- conflito sobre compatibilidade entre duas revisões: deixar a compatibilidade pendente;
- continuar pesquisando BIOS, alimentação, vídeo, charger, EC/KBC, RAM, USB, HDMI, casos reais, vídeos e datasheets.

## Ordem atual

Projeto: TECNICOPROMAX
Missão: MISSAO_001_LA-B071P
Board: LA-B071P

O executor deve começar a pesquisa real IMEDIATAMENTE, mesmo com `MCP_PENDING=YES`.

Não aguardar o usuário abrir o YI Workstation para iniciar coleta de evidências.

Não exigir validação MCP como pré-condição para pesquisa.

Quando o MCP estiver disponível, enviar o primeiro DEBATE com o material já coletado.

## Estado esperado

Enquanto houver pesquisa útil:

PROJECT=TECNICOPROMAX
MISSION=MISSAO_001_LA-B071P
STATUS=WORKING
MCP_PENDING=YES ou NO

Somente usar `BLOCKED` se não houver nenhuma tarefa independente segura restante.