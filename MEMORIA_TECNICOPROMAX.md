# TECNICOPROMAX — MEMÓRIA MESTRA DO PROJETO

## 1. OBJETIVO DO PROJETO

O **Técnico Pro Max** será uma base técnica organizada para diagnóstico e reparo de placas-mãe de notebooks e, futuramente, outros equipamentos eletrônicos reparáveis em bancada.

A ideia central é transformar pesquisas dispersas em fóruns, sites técnicos, vídeos, esquemas, boardviews, manuais, bancos de reparo, posts em vários idiomas e experiências documentadas em uma base única, pesquisável, traduzida para português e organizada por **marca > modelo do equipamento > modelo/board ID da placa-mãe > sintoma/problema**.

O ChatGPT atua como **engenheiro/arquiteto do projeto e auditor técnico**. O OpenCode atua como **executor**, criando estrutura, pesquisando, organizando, escrevendo arquivos, atualizando índices e mantendo a base consistente.

O projeto deve sempre privilegiar precisão, rastreabilidade e utilidade prática de bancada.

---

## 2. PRINCÍPIO MAIS IMPORTANTE: NÃO INVENTAR

Nenhuma informação técnica específica de uma placa pode ser criada por inferência sem deixar isso explícito.

Regras obrigatórias:

- Nunca inventar board ID, revisão de placa, CI, tensão, rail, sequência de power, resistência, sinal lógico, BIOS, EC/KBC, MOSFET, charger, Super I/O ou componente específico.
- Nunca assumir que duas placas do mesmo notebook são iguais.
- Nunca assumir que um mesmo modelo de notebook utiliza uma única placa-mãe.
- Nunca copiar uma solução de uma revisão de placa para outra sem verificar compatibilidade.
- Se o usuário fornecer apenas o modelo comercial do notebook, primeiro identificar as possíveis placas usadas naquele modelo.
- Se o usuário fornecer o board ID da placa, ele passa a ser o identificador técnico principal da pesquisa.
- Qualquer informação não confirmada deve ser marcada como **HIPÓTESE**, **RELATO DE TERCEIRO**, **POSSÍVEL**, **NÃO CONFIRMADO** ou equivalente.
- Nunca transformar um relato isolado em procedimento oficial.

Exemplo: se o usuário citar “Dell Latitude 3450 / LA-B071P” apenas como exemplo, esse vínculo não deve ser tratado como verdadeiro sem verificação documental.

---

## 3. IDENTIFICADOR PRINCIPAL DO EQUIPAMENTO

A pesquisa pode começar por dois caminhos.

### Caminho A — modelo do notebook

Exemplo conceitual:

- Marca: Dell
- Família: Latitude
- Modelo comercial: 3450

Nesse caso, o executor deve primeiro pesquisar quais placas-mãe foram usadas nesse modelo, incluindo variações por:

- revisão;
- CPU;
- GPU dedicada/integrada;
- região;
- lote;
- geração;
- fabricante ODM;
- board ID;
- código de PCB;
- part number OEM.

Depois disso, criar uma subpasta para cada placa efetivamente confirmada.

### Caminho B — board ID / código da placa

Esse é o caminho preferencial quando disponível.

Exemplos de identificadores que podem aparecer nas placas:

- LA-XXXXP;
- DA0XXXXMBXX;
- NM-XXXX;
- XXXXXX-XXX;
- códigos Compal, Quanta, Wistron, Inventec, Pegatron, Clevo, Lenovo, Dell, HP, Acer, ASUS, etc.

O executor deve preservar o código exatamente como impresso na placa e registrar também revisões como REV:1.0, REV:A00, VER:1.1, etc.

---

## 4. ESTRUTURA PADRÃO DA BASE

Estrutura prevista:

```text
TECNICOPROMAX/
│
├── MEMORIA_TECNICOPROMAX.md
├── README.md
│
├── BASE_GERAL/
│   ├── 00_Metodologia/
│   ├── 01_Sintomas_Gerais/
│   ├── 02_Testes_de_Bancada/
│   ├── 03_Fontes_e_Rails/
│   ├── 04_BIOS_EC_KBC/
│   ├── 05_Carregamento_Bateria/
│   ├── 06_CPU_GPU_RAM/
│   ├── 07_Display_e_Video/
│   ├── 08_Perifericos/
│   ├── 09_Termica_e_Consumo/
│   └── 10_Instrumentacao/
│
├── MARCAS/
│   ├── DELL/
│   │   └── Latitude_3450/
│   │       ├── 00_MODELO_GERAL.md
│   │       ├── PLACAS/
│   │       │   ├── BOARD_ID_A/
│   │       │   │   ├── 00_IDENTIFICACAO.md
│   │       │   │   ├── 01_VARIANTES_E_REVISOES.md
│   │       │   │   ├── 02_SINTOMAS_CONHECIDOS.md
│   │       │   │   ├── 03_DIAGNOSTICO_GERAL.md
│   │       │   │   ├── PROBLEMAS/
│   │       │   │   │   ├── NAO_LIGA.md
│   │       │   │   │   ├── SEM_VIDEO.md
│   │       │   │   │   ├── NAO_CARREGA.md
│   │       │   │   │   └── ...
│   │       │   │   ├── BIOS/
│   │       │   │   ├── ESQUEMAS_BOARDVIEW/
│   │       │   │   ├── VIDEOS.md
│   │       │   │   ├── FONTES.md
│   │       │   │   └── REVISOES.md
│   │       │   └── BOARD_ID_B/
│   │       └── ...
│   ├── HP/
│   ├── LENOVO/
│   ├── ACER/
│   ├── ASUS/
│   └── OUTRAS/
│
└── INDICES/
    ├── INDICE_MARCAS.md
    ├── INDICE_MODELOS.md
    ├── INDICE_PLACAS.md
    ├── INDICE_SINTOMAS.md
    └── INDICE_COMPONENTES.md
```

A estrutura pode evoluir, mas a hierarquia **marca > modelo do equipamento > board ID/revisão > sintoma** deve ser preservada.

---

## 5. BASE GERAL — TAXONOMIA DE PROBLEMAS

Antes de pesquisar placas específicas, o projeto deve construir uma base genérica de sintomas e testes aplicáveis a placas de notebooks.

Não existe uma lista verdadeiramente finita de “todos os defeitos possíveis”, mas a base deve cobrir pelo menos as classes abaixo e ser expandida sempre que um novo caso real aparecer.

### Alimentação e partida

- completamente morto / não liga;
- consumo zero na fonte;
- consumo muito baixo e estático;
- consumo alto imediato;
- curto na entrada;
- curto em rail secundário;
- liga e desliga imediatamente;
- liga por alguns segundos e desliga;
- entra em proteção;
- fonte externa cai;
- LED acende mas placa não parte;
- botão power sem resposta;
- power sequence incompleta;
- ausência de rails always-on;
- ausência de 3V/5V de standby;
- falha no sinal ACOK;
- falha em RSMRST#;
- falha em SLP_S3#/SLP_S4#/SLP_S5#;
- falha em ALL_SYS_PWRGD / SUS_PWRGD / sinais equivalentes;
- falha em clock/reset.

### Vídeo e POST

- liga e não dá vídeo;
- sem vídeo interno e externo;
- vídeo somente externo;
- vídeo somente interno;
- tela branca;
- tela preta com backlight;
- sem backlight;
- imagem fraca;
- imagem com artefatos;
- imagem distorcida;
- trava no POST;
- reinicia durante POST;
- bipes/códigos de LED;
- POST lento;
- não reconhece painel;
- falha em eDP/LVDS;
- falha de GPU dedicada;
- falha de iGPU/PCH/CPU quando aplicável.

### BIOS / EC / KBC / firmware

- BIOS corrompida;
- firmware incompatível;
- região Intel ME com problema;
- EC firmware corrompido;
- KBC/EC sem atividade;
- SPI sem comunicação;
- BIOS sem alimentação;
- BIOS sem CLK/CS/DATA;
- equipamento liga após regravação e volta a falhar;
- equipamento demora para iniciar;
- equipamento não desliga corretamente;
- perda de serial/DMI/UUID/MAC após gravação;
- falha de CMOS/RTC.

### CPU / PCH / GPU

- CPU sem alimentação;
- VRM da CPU ausente;
- VCORE anormal;
- VCCSA/VCCIO ausentes quando aplicáveis;
- CPU aquecendo instantaneamente;
- CPU totalmente fria;
- falha BGA;
- PCH aquecendo excessivamente;
- GPU dedicada em curto;
- VRAM defeituosa;
- GPU com artefatos;
- defeito de solda/BGA;
- falha de enable/PWM do VRM.

### Memória

- não reconhece RAM;
- reconhece apenas um slot;
- liga sem POST;
- reinicia com memória instalada;
- slot com dano físico;
- tensão DDR ausente;
- VTT/VREF incorretos;
- trilha rompida;
- controlador de memória defeituoso;
- memória soldada com falha.

### Carregamento e bateria

- não reconhece carregador;
- não reconhece bateria;
- não carrega bateria;
- carrega intermitente;
- carrega somente desligado;
- descarrega mesmo conectado;
- charger IC não habilita MOSFETs;
- gate de entrada incorreto;
- ACDET/ACOK anormal;
- BATDRV ausente;
- MOSFET de entrada em curto/aberto;
- resistor shunt alterado;
- charger aquecendo;
- bateria não comunica via SMBus;
- identificação de carregador ausente quando o fabricante utiliza linha de ID.

### Armazenamento

- SSD/HDD não reconhecido;
- NVMe não reconhecido;
- SATA ausente;
- boot intermitente;
- slot M.2 sem alimentação;
- falha de clock/reset no armazenamento;
- curto em linha de alimentação do dispositivo.

### USB / portas / I/O

- USB sem alimentação;
- USB sem dados;
- USB derruba a placa;
- USB-C sem negociação;
- USB-C sem carregamento;
- HDMI/DisplayPort sem sinal;
- áudio ausente;
- microfone ausente;
- Ethernet ausente;
- Wi-Fi/Bluetooth ausente;
- teclado/touchpad não funcionam;
- webcam ausente;
- leitor SD ausente;
- porta DC danificada.

### Temperatura, fan e estabilidade

- superaquecimento;
- fan não gira;
- fan gira no máximo;
- thermal throttling;
- desliga por temperatura;
- sensor térmico ausente;
- equipamento congela;
- reinicia sob carga;
- falha somente frio/quente;
- consumo anormal em idle;
- bateria drenando rapidamente.

### Danos físicos

- oxidação;
- líquido;
- carbonização;
- trilha rompida;
- pad arrancado;
- conector quebrado;
- solda fria;
- componente faltando;
- componente deslocado;
- PCB empenada;
- dano por queda;
- dano por fonte/carregador inadequado;
- ESD;
- sobretensão.

### Casos especiais

- liga sozinho ao conectar fonte;
- só liga sem bateria;
- só liga com bateria;
- só liga após várias tentativas;
- só liga depois de aquecer/esfriar;
- desliga ao conectar periférico;
- sleep/hibernação não retorna;
- consumo em standby excessivo;
- RTC resetando;
- data/hora não salva;
- erro de TPM;
- erro de firmware de segurança;
- erro de teclado no POST;
- board reconhecida como modelo diferente após gravação.

---

## 6. TESTES GERAIS DE BANCADA

A base geral deve documentar os testes abaixo com procedimento, finalidade, instrumento, risco, resultado esperado e interpretação.

### Inspeção e preparação

- inspeção visual com aumento;
- procura por oxidação, carbonização, trincas e componentes deslocados;
- checagem de conectores;
- conferência de parafusos/isoladores;
- limpeza adequada;
- inspeção térmica quando aplicável.

### Fonte assimétrica

Registrar:

- tensão de entrada correta para o equipamento;
- limite de corrente seguro;
- consumo ao conectar;
- consumo ao pressionar power;
- padrão de subida/queda de corrente;
- consumo estático;
- consumo cíclico;
- indícios de curto.

Nunca recomendar bypass de proteção apenas para “fazer ligar”.

### Resistência para GND

- medir rails principais;
- comparar leituras entre placas/referências quando houver fonte confiável;
- distinguir rail naturalmente de baixa resistência de curto real;
- registrar escala do multímetro e condição da placa.

### Injeção de tensão

Quando aplicável, documentar:

- rail alvo;
- tensão máxima segura conhecida;
- corrente limitada;
- motivo do teste;
- método de localização térmica;
- risco de danificar CPU/PCH/GPU/EC se tensão inadequada for usada.

Não recomendar tensão de injeção específica sem base técnica para aquele rail.

### Sequência de alimentação

Checar, quando documentado para a plataforma:

- entrada DC;
- MOSFETs de proteção;
- VIN principal;
- 3V/5V always;
- alimentação EC/KBC;
- RTC;
- BIOS;
- sinais de reset;
- botão power;
- SLP signals;
- enables;
- power good;
- VCORE e demais VRMs;
- clock;
- POST.

### BIOS/SPI

- alimentação do chip;
- atividade CLK/CS/DATA;
- leitura e backup antes de gravação;
- comparação de tamanho do dump;
- preservação de dados únicos;
- checagem de regiões quando aplicável;
- verificação pós-gravação.

### Osciloscópio / analisador lógico

Quando necessário:

- clocks;
- SPI;
- resets;
- PWM;
- sinais digitais críticos;
- comunicação SMBus/I2C;
- atividade em linhas de dados.

---

## 7. PESQUISA POR PLACA

Para cada board ID, o executor deve fazer pesquisas em múltiplos idiomas e formatos.

### Consultas básicas

Pesquisar combinações como:

```text
"BOARD-ID" no power
"BOARD-ID" no display
"BOARD-ID" dead motherboard
"BOARD-ID" not charging
"BOARD-ID" short circuit
"BOARD-ID" bios
"BOARD-ID" schematic
"BOARD-ID" boardview
"BOARD-ID" repair
"BOARD-ID" power sequence
"BOARD-ID" charger
"BOARD-ID" EC
"BOARD-ID" KBC
"BOARD-ID" no boot
"BOARD-ID" no POST
"BOARD-ID" current draw
```

E equivalentes em português, espanhol, russo, hindi/inglês técnico, chinês quando viável, vietnamita, turco e outros idiomas relevantes.

### Pesquisa por componente

Se aparecer um CI relevante, pesquisar também:

```text
"BOARD-ID" "PUxxx"
"BOARD-ID" "PQxxx"
"BOARD-ID" "Uxxx"
"BOARD-ID" "charger IC model"
"BOARD-ID" "EC model"
"BOARD-ID" "PWM model"
```

### Pesquisa por sintoma específico

Cada sintoma conhecido deve gerar uma pesquisa independente.

Exemplo conceitual:

```text
BOARD-ID + no power
BOARD-ID + 19V present no 3V 5V
BOARD-ID + no display
BOARD-ID + fan spin no display
BOARD-ID + battery not charging
```

O objetivo é evitar que um único artigo ou vídeo seja tratado como representação completa do defeito.

---

## 8. TIPOS DE FONTES

Prioridade sugerida:

1. documentação oficial do fabricante;
2. datasheets oficiais dos componentes;
3. esquemas e boardviews obtidos por meios legítimos;
4. manuais de serviço;
5. fóruns técnicos especializados;
6. artigos de reparo detalhados;
7. vídeos técnicos com medições demonstradas;
8. comunidades de técnicos;
9. relatos individuais;
10. posts sem evidência técnica.

Quanto menor a qualidade da fonte, menor deve ser o nível de confiança atribuído à informação.

Não remover uma informação útil apenas porque vem de fórum ou vídeo; classificar corretamente sua confiabilidade.

---

## 9. TRADUÇÃO PARA PORTUGUÊS

Toda informação útil encontrada em outro idioma deve ser traduzida para português.

Regras:

- preservar nomes de sinais;
- preservar designadores de componentes;
- preservar board IDs;
- preservar nomes de rails;
- preservar códigos de erro;
- preservar nomes de CIs;
- não “traduzir” siglas técnicas indevidamente;
- quando um termo puder gerar dúvida, manter o original entre parênteses.

Exemplo:

```text
Ausência da tensão de espera de 3,3 V (3.3V_ALW / +3VALW, conforme a nomenclatura do esquema).
```

A tradução deve transmitir o conteúdo técnico, não fazer tradução literal ruim.

---

## 10. ARQUIVO DE CADA PROBLEMA

Cada problema deve ter seu próprio arquivo Markdown.

Template obrigatório:

```markdown
# [SINTOMA / PROBLEMA]

## Identificação
- Marca:
- Modelo do equipamento:
- Board ID:
- Revisão:
- Variante:

## Sintoma
Descrição objetiva do comportamento observado.

## Causas documentadas
1. ...
2. ...
3. ...

## Ordem de diagnóstico recomendada
1. ...
2. ...
3. ...

## Pontos de medição
| Ponto / Rail / Sinal | Condição | Valor esperado | Valor encontrado na fonte | Observação |
|---|---|---|---|---|

Somente preencher valores esperados quando houver fonte confiável.

## Componentes relacionados
- ...

## Procedimentos encontrados
### Procedimento 1
- Fonte:
- Resumo traduzido:
- Condições em que se aplica:
- Resultado relatado:
- Nível de confiança:

### Procedimento 2
...

## Casos reais encontrados
### Caso 1
- Sintoma:
- Diagnóstico:
- Componente/causa:
- Reparo:
- Resultado:
- Fonte:

## Vídeos relacionados
- Título — URL — idioma — observação

## Observações e riscos
- ...

## Fontes
1. Título — autor/site — URL — data de acesso
2. ...

## Status
- [ ] Pesquisa inicial
- [ ] Pesquisa multilíngue
- [ ] Fontes cruzadas
- [ ] Tradução revisada
- [ ] Auditoria técnica
- [ ] Validado em bancada
```

---

## 11. FONTES NO FINAL DE TODO ARQUIVO

Nenhum arquivo técnico pode ser concluído sem referências.

Para cada fonte registrar, quando disponível:

- título;
- site/canal/fórum;
- autor;
- URL;
- idioma original;
- data de acesso;
- tipo de fonte;
- resumo do que ela sustenta;
- nível de confiança.

Exemplo:

```text
Fonte 03
Título: Repair case — board XXXXX no power
Site: Example Repair Forum
Idioma: Inglês
URL: https://...
Acessado em: 2026-09-12
Uso: sustenta o relato de falha no charger IC em caso específico.
Confiança: média — relato técnico com medições, mas sem documentação oficial.
```

---

## 12. VÍDEOS

O projeto não precisa transcrever todos os vídeos.

Quando um vídeo for relevante:

- salvar título;
- URL;
- canal;
- idioma;
- duração, se disponível;
- sintoma abordado;
- observação curta sobre a utilidade.

Se a informação técnica do vídeo for usada no texto, ela deve ser tratada como fonte e, idealmente, confirmada por outra fonte quando possível.

---

## 13. NÍVEIS DE CONFIANÇA

Toda solução específica deve poder receber uma classificação.

### ALTA

- documentação oficial;
- datasheet;
- esquema;
- medição reproduzível;
- múltiplas fontes independentes concordantes.

### MÉDIA

- fórum técnico detalhado;
- vídeo com medições claras;
- caso de reparo bem documentado;
- uma fonte técnica plausível sem confirmação independente.

### BAIXA

- comentário isolado;
- post sem medição;
- relato incompleto;
- informação replicada sem fonte original.

Nunca apresentar confiança baixa como fato definitivo.

---

## 14. DIFERENÇA ENTRE CAUSA, SINTOMA E SOLUÇÃO

O projeto deve manter essas três coisas separadas.

Exemplo:

- Sintoma: não liga.
- Causa: curto em rail de 3,3 V.
- Causa raiz: capacitor cerâmico em curto.
- Diagnóstico: baixa resistência para GND + injeção controlada + localização térmica.
- Solução: substituição do capacitor defeituoso.

Isso evita arquivos do tipo “não liga = trocar CI X”, que são tecnicamente pobres e perigosos.

---

## 15. MESMO SINTOMA, MÚLTIPLAS CAUSAS

Nunca associar um sintoma a uma única causa.

Exemplo: “não liga” pode envolver:

- conector;
- fonte;
- MOSFETs de entrada;
- charger;
- curto;
- 3V/5V standby;
- EC/KBC;
- BIOS;
- clock;
- reset;
- PCH;
- CPU;
- trilha rompida;
- oxidação;
- resistor alterado;
- sinal de enable;
- botão power;
- firmware;
- outros.

A base deve sempre priorizar **árvore de diagnóstico**, não “troca de peça por tentativa”.

---

## 16. ÍNDICE DE COMPONENTES

Sempre que um CI importante aparecer em uma pesquisa de placa, registrar no índice:

- part number;
- fabricante;
- função;
- placas em que foi encontrado;
- datasheet;
- sintomas associados;
- designadores usuais;
- observações.

Isso permitirá pesquisar futuramente por CI mesmo sem saber o modelo da placa.

---

## 17. REVISÕES DE PLACA

Uma mesma board ID pode possuir revisões diferentes.

Exemplo conceitual:

```text
BOARD-XXXX
├── REV_1.0
├── REV_1.1
└── REV_2.0
```

Se uma solução for confirmada apenas para REV 1.0, isso deve ficar explícito.

Nunca aplicar automaticamente a REV 1.0 à REV 2.0.

---

## 18. MODELO COMERCIAL COM VÁRIAS PLACAS

O arquivo `00_MODELO_GERAL.md` deve conter uma tabela como:

| Modelo comercial | Board ID | Revisão | CPU | GPU | Evidência | Fonte |
|---|---|---|---|---|---|---|

Somente incluir combinações confirmadas.

---

## 19. CONTROLE DE DUPLICIDADE

Antes de criar nova pasta:

1. procurar board ID existente;
2. procurar aliases;
3. procurar revisão;
4. verificar se o mesmo modelo já está cadastrado com grafia diferente;
5. evitar pastas duplicadas.

Exemplos de normalização:

- `LA-B071P`
- `LAB071P`

Podem ser indexados como aliases, mas a pasta deve usar a grafia oficial impressa/documentada.

---

## 20. AUDITORIA DE PESQUISA

Cada placa deve possuir `REVISOES.md` contendo:

- data da pesquisa;
- executor/modelo de IA usado;
- consultas realizadas;
- novas fontes;
- arquivos alterados;
- informação adicionada;
- informação removida;
- motivo da alteração;
- conflitos encontrados;
- itens ainda não confirmados.

Nunca apagar silenciosamente informação antiga. Se estiver errada, registrar correção.

---

## 21. FLUXO QUANDO O USUÁRIO INFORMAR UM NOVO EQUIPAMENTO

### Se informar apenas notebook

Exemplo:

```text
Dell Latitude XXXX
```

Executar:

1. identificar modelo exato;
2. levantar board IDs possíveis;
3. apresentar lista ao usuário quando houver ambiguidade relevante;
4. criar estrutura do modelo;
5. criar subpastas somente para placas confirmadas;
6. pesquisar problemas por placa.

### Se informar board ID

Exemplo:

```text
LA-XXXXP
```

Executar:

1. identificar fabricante/ODM;
2. identificar equipamentos que usam a placa;
3. identificar revisões;
4. criar pasta técnica da placa;
5. levantar sintomas documentados;
6. pesquisar cada sintoma separadamente;
7. montar árvore de diagnóstico;
8. registrar fontes.

### Se informar board ID + sintoma

Exemplo:

```text
LA-XXXXP — não liga
```

Priorizar esse defeito, mas ainda registrar informações essenciais de identificação da placa.

---

## 22. PESQUISA INCREMENTAL

O Técnico Pro Max não precisa terminar uma placa inteira de uma vez.

O conteúdo pode evoluir por camadas:

- versão 0.1: identificação;
- versão 0.2: principais sintomas;
- versão 0.3: pesquisa multilíngue;
- versão 0.4: esquemas/datasheets;
- versão 0.5: vídeos;
- versão 0.6: casos reais;
- versão 1.0: auditado e consolidado.

Isso permite começar a usar a base imediatamente e melhorá-la continuamente.

---

## 23. SEGURANÇA DE BANCADA

Toda instrução deve assumir que o operador é técnico, mas ainda assim preservar boas práticas.

- usar ESD;
- conferir polaridade;
- usar fonte com limite de corrente;
- não injetar tensão desconhecida em rail desconhecido;
- não remover proteções de entrada sem diagnóstico;
- não substituir fusível por jumper como “solução”;
- verificar curto antes de energizar;
- preservar dump original de BIOS;
- evitar aquecimento/reflow sem diagnóstico;
- diferenciar teste temporário de reparo definitivo.

---

## 24. REGRAS DE QUALIDADE PARA O EXECUTOR

O OpenCode deve:

- pesquisar antes de escrever afirmação específica;
- traduzir para português;
- citar fonte;
- comparar fontes;
- sinalizar conflitos;
- não inventar valores;
- não preencher lacunas para “deixar bonito”;
- não apagar informação sem registrar;
- preservar URL original;
- preferir Markdown;
- manter arquivos legíveis por humanos;
- atualizar índices após novas placas;
- trabalhar por etapas;
- fazer commit lógico por placa ou conjunto de mudanças;
- parar e pedir orientação quando houver ambiguidade crítica.

---

## 25. PAPEL DO CHATGPT E DO OPENCODE

### ChatGPT — engenheiro / auditor

Responsável por:

- definir arquitetura;
- definir metodologia;
- criar prompts de execução;
- auditar conteúdo produzido;
- detectar inconsistências;
- melhorar a estratégia de pesquisa;
- decidir mudanças estruturais;
- revisar qualidade técnica;
- evitar alucinação e extrapolação indevida.

### OpenCode — executor

Responsável por:

- criar pastas;
- criar arquivos;
- fazer pesquisas;
- traduzir;
- organizar fontes;
- manter índices;
- registrar revisões;
- implementar ferramentas de apoio;
- executar tarefas repetitivas definidas pelo engenheiro.

---

## 26. PROMPT BASE PARA O OPENCODE

Usar como regra operacional permanente:

```text
VOCÊ É O EXECUTOR DO PROJETO TECNICOPROMAX.

Antes de qualquer tarefa, leia integralmente:
MEMORIA_TECNICOPROMAX.md

O ChatGPT atua como engenheiro/arquiteto e você atua como executor.

OBJETIVO:
Construir e manter uma base técnica profissional de diagnóstico e reparo de placas-mãe, organizada por marca, modelo de equipamento, board ID/revisão e sintoma.

REGRAS ABSOLUTAS:
- NÃO inventar dados técnicos.
- NÃO inventar board ID.
- NÃO inventar tensão.
- NÃO inventar componente.
- NÃO inventar sequência de power.
- NÃO inventar compatibilidade entre placas.
- NÃO tratar relato isolado como fato.
- TODA informação técnica específica precisa de fonte.
- Traduzir conteúdo útil para português.
- Preservar nomes técnicos, sinais, rails, designadores e códigos originais.
- Registrar as fontes no final do arquivo.
- Diferenciar fato, hipótese, relato e procedimento confirmado.
- Diferenciar sintoma, causa, diagnóstico e solução.

Quando receber um modelo de notebook:
1. identificar todas as placas-mãe confirmadas usadas nele;
2. criar uma pasta por board ID/revisão;
3. não misturar soluções entre placas diferentes;
4. pesquisar os sintomas documentados de cada placa.

Quando receber diretamente um board ID:
1. identificar fabricante/ODM e equipamentos relacionados;
2. identificar revisões;
3. pesquisar em múltiplos idiomas;
4. pesquisar todos os sintomas encontrados;
5. criar um arquivo separado por sintoma relevante;
6. montar árvore de diagnóstico baseada em evidências;
7. registrar casos reais, medições e soluções encontradas;
8. salvar vídeos relevantes como links;
9. registrar fontes completas;
10. atualizar os índices globais.

PESQUISA:
Pesquisar web, fóruns técnicos, documentação, datasheets, manuais de serviço, esquemas/boardviews obtidos legitimamente, artigos, vídeos e comunidades técnicas.

Sempre cruzar fontes quando possível.

Se duas fontes entrarem em conflito, NÃO escolher arbitrariamente.
Registrar o conflito e o contexto de cada uma.

Se faltar evidência:
marcar como NÃO CONFIRMADO e continuar pesquisando.

Ao concluir cada tarefa, entregar relatório contendo:
- pastas criadas;
- arquivos criados;
- arquivos atualizados;
- board IDs identificados;
- revisões identificadas;
- sintomas levantados;
- quantidade de fontes consultadas;
- idiomas pesquisados;
- conflitos encontrados;
- itens não confirmados;
- próximos passos recomendados.
```

---

## 27. PRIMEIRA ETAPA DO PROJETO

Antes de cadastrar o primeiro notebook, o executor deverá criar a estrutura inicial e a **BASE_GERAL**.

A primeira etapa deverá:

1. criar pastas-base;
2. criar índices;
3. consolidar a taxonomia geral de sintomas;
4. criar metodologia de diagnóstico;
5. criar templates;
6. criar política de fontes;
7. criar política de tradução;
8. criar sistema de confiança;
9. criar controle de revisões;
10. criar checklist de pesquisa por placa.

Somente depois iniciar modelos específicos.

---

## 28. VISÃO FUTURA

A estrutura em Markdown deve permitir futuramente criar uma aplicação com:

- busca por marca;
- busca por notebook;
- busca por board ID;
- busca por CI;
- busca por sintoma;
- árvore interativa de diagnóstico;
- filtro por revisão;
- fotos da placa;
- localização de componente;
- integração com esquema/boardview;
- histórico de reparos da empresa;
- anexação de medições próprias;
- ranking de causas mais frequentes;
- base de BIOS validadas;
- checklist de bancada;
- geração de ordem técnica;
- IA consultando somente a base validada.

O Markdown é a fonte inicial de verdade; uma interface ou banco estruturado poderá ser criado depois sem perder o histórico.

---

## 29. REGRA DE EVOLUÇÃO DESTA MEMÓRIA

Este arquivo é a memória mestra do Técnico Pro Max.

Sempre que o usuário definir uma nova regra permanente do projeto:

1. atualizar este arquivo;
2. registrar a mudança em controle de versão;
3. não apagar premissas anteriores sem justificativa;
4. manter compatibilidade com a estrutura existente sempre que possível.

---

## 30. ESTADO ATUAL

- Repositório: `patricsouza22-gif/TECNICOPROMAX`
- Fase: definição de arquitetura e metodologia.
- Conteúdo técnico de placas específicas: ainda não iniciado.
- Próxima ação recomendada: criar a estrutura inicial do repositório e a BASE_GERAL antes de cadastrar a primeira placa.
