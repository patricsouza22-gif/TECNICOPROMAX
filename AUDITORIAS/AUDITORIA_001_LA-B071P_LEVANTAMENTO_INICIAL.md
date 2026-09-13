# AUDITORIA 001 — LA-B071P — LEVANTAMENTO INICIAL

## Decisão

`REQUEST_EVIDENCE`

O levantamento inicial é útil como ponto de partida, mas ainda NÃO pode ser consolidado como base técnica da LA-B071P.

## Motivos principais

1. A pesquisa ainda está concentrada em fontes comerciais e em um único idioma.
2. Diversos componentes e características foram marcados como inferidos de plataforma/modelo, o que viola a regra de não preencher lacunas por inferência.
3. Vários sintomas estão referenciados apenas à própria Missão 001, portanto ainda precisam voltar às fontes primárias/originais.
4. A associação Latitude 3450/3550 deve permanecer preliminar até ser sustentada por documentação técnica mais forte, fotos/board ID, service material ou múltiplas fontes técnicas independentes.
5. A estrutura e os commits relatados não aparecem no repositório GitHub atual; portanto, do ponto de vista do repositório remoto, eles não podem ser considerados executados.
6. O critério de “15 fontes” não deve substituir qualidade. Uma fonte técnica forte vale mais que vários anúncios replicados.

## Itens que NÃO podem entrar como fatos confirmados neste estágio

Até obter fonte direta/esquema/boardview/service documentation ou evidência equivalente, manter como `NÃO CONFIRMADO` ou remover do conteúdo canônico:

- chip SPI de 8–16 MiB “tipo Winbond ou Macronix”;
- codec Realtek ALC32xx;
- Ethernet Intel I218-LM ou semelhante;
- Wi-Fi M.2 Intel Dual Band Wireless-AC;
- armazenamento SATA + M.2/NGFF;
- conector de tela eDP;
- jack 7,4 × 5,0 mm;
- qualquer conclusão do tipo “a CPU encontrada implica consequentemente os demais subsistemas típicos”.

Também não consolidar `MEC5085 / ECE1099` como um único componente confirmado. Identificar o componente real por revisão/variante.

## Correção de classificação das fontes

Anúncios de eBay, Amazon, AliExpress e Newegg podem servir como pista de compatibilidade, fotos, part numbers e palavras-chave, mas não devem ser usados isoladamente como prova elétrica, de revisão, power sequence ou componente.

A Missão 001 é documento interno do projeto e NÃO deve ser contada como fonte técnica independente para provar um sintoma.

## Pesquisa obrigatória antes da consolidação

Prioridade máxima:

1. localizar schematic real da LA-B071P e registrar título, revisão, páginas relevantes e origem;
2. localizar boardview/fotos legíveis da PCB para confirmar designadores e revisão;
3. identificar charger IC, PWM 3V/5V, EC/KBC, chips SPI e demais CIs diretamente no esquema/boardview/foto;
4. reconstruir a sequência de alimentação usando somente sinais/designadores documentados;
5. voltar a cada sintoma da Missão 001 e localizar a URL/thread/fonte primária que originou o caso;
6. pesquisar fóruns técnicos em inglês, russo, polonês, vietnamita, espanhol, turco, chinês e comunidades indianas;
7. separar em cada caso: sintoma, medição, hipótese, causa confirmada, reparo e resultado;
8. localizar datasheets dos CIs efetivamente confirmados;
9. pesquisar BIOS/ME/EC separadamente por revisão e CPU/plataforma;
10. verificar se LA-B071P e LA-B072P compartilham partes do esquema sem assumir equivalência total.

## Regra de escrita no repositório

Não usar frases como “estrutura criada em memória” ou “commit simulado” como se fossem execução.

Se a tarefa disser para criar arquivos/commits:

- escrever os arquivos no workspace real;
- executar `git status`;
- executar `git add` e `git commit` quando autorizado;
- registrar o SHA real;
- fazer push para o repositório quando o fluxo vigente exigir;
- se não houver permissão/acesso, declarar `NÃO EXECUTADO`.

Nunca relatar commit inexistente como concluído.

## Próximo debate com o ChatGPT

O próximo pacote de debate deve trazer evidência, não plano.

Enviar:

- URLs diretas das fontes primárias;
- pelo menos 1 esquema/boardview ou declarar explicitamente que não foi localizado;
- tabela de componentes com coluna `CONFIRMADO POR`;
- tabela de sintomas com `FONTE PRIMÁRIA`, `CAUSA CONFIRMADA?`, `REPARO CONFIRMADO?`;
- conflitos encontrados;
- itens removidos por serem inferência;
- diff/arquivos reais criados;
- SHAs reais de commits, se houver.

## Estado

- `STATUS_RESEARCH: EM_ANDAMENTO`
- `READY_FOR_CONSOLIDATION: NO`
- `READY_FOR_CHATGPT_DEBATE: YES — somente para auditoria metodológica e pedido de evidência`
- `DECISION: REQUEST_EVIDENCE`
