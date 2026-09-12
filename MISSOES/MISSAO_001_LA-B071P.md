# MISSAO 001 — COMPAL LA-B071P

## Objetivo

Levantar, organizar e documentar de forma abrangente os problemas que a placa-mãe Compal LA-B071P pode apresentar, separando rigorosamente:

1. problemas efetivamente documentados para LA-B071P;
2. problemas plausíveis por subsistema, mas ainda não documentados especificamente para esta placa;
3. causas confirmadas;
4. hipóteses;
5. sintomas;
6. procedimentos de diagnóstico;
7. reparos relatados;
8. informações de firmware/BIOS/ME/EC;
9. variantes e revisões.

O objetivo não é criar uma lista genérica e chamar de específica. A missão deve buscar evidência real para a LA-B071P e, em paralelo, montar uma matriz completa de falhas possíveis por subsistema.

---

## Situação atual

Esta missão SUBSTITUI a restrição antiga que dizia para não iniciar o TECNICOPROMAX enquanto a integração YI Workstation/FCC estivesse em andamento.

A partir desta missão, o escopo ativo é TECNICOPROMAX.

NÃO modificar a integração YI/FCC nesta missão.

O OpenCode/Ben Code deve atuar como executor/pesquisador. O ChatGPT atua como comandante técnico, engenheiro, arquiteto e auditor.

Antes de executar, ler:

- `MEMORIA_TECNICOPROMAX.md`
- `PROTOCOLO_CONVERSACAO_MCP.md`
- `PROTOCOLO_TRABALHO_CONTINUO.md`
- este arquivo

---

## Identificação inicial já encontrada

Identificação preliminar a confirmar com múltiplas fontes:

- Board ID: LA-B071P
- Plataforma/Projeto: ZAL50 / ZAL51 / ZAL60 / ZAL61
- ODM: Compal
- Equipamentos associados em fontes técnicas: Dell Latitude 3450 e Dell Latitude 3550
- Há referência de esquema compartilhado com LA-B072P em algumas fontes; NÃO assumir equivalência total entre as placas sem comparação documental.
- Revisões mencionadas em fontes: REV 1.0 / A00 e referências de BIOS A07/A08; separar revisão física de PCB de versão de BIOS.
- CPU/PCH em material de esquema: Intel Haswell/Broadwell ULT-U, DDR3, variante UMA em pelo menos parte da documentação.
- EC/SIO citado em fonte de esquema: SMSC MEC5085/ECE1099. Confirmar variante exata por revisão antes de consolidar.

---

## Problemas específicos já encontrados em pesquisa preliminar

Estes itens são PONTOS DE PARTIDA e não conclusões finais.

### 1. Não liga / sem LED ao conectar carregador

Há relato específico de LA-B071P que parou em uso e passou a não ligar, sem LED ao conectar o carregador.

Classificação inicial: sintoma documentado, causa não confirmada.

### 2. Liga / power on, mas sem vídeo

Há múltiplos relatos específicos de LA-B071P com `power on but no display` / `no display`.

Em vários casos houve tentativa de reconstrução/limpeza de BIOS/ME. Isso NÃO prova que BIOS/ME seja a causa em todos os casos.

Classificação inicial: sintoma fortemente documentado; causas múltiplas possíveis.

### 3. Vídeo muito demorado / late display

Há caso específico marcado como `very late display`; após intervenção de firmware/ME o usuário relatou sucesso.

Classificação inicial: caso documentado relacionado a firmware/ME, ainda precisa de validação cruzada.

### 4. Cinco bipes + sem vídeo

Há casos específicos de LA-B071P com cinco bipes e ausência de vídeo.

Uma fonte associa cinco bipes a falha RTC/CMOS, mas pelo menos um caso permaneceu sem vídeo após substituição/ação de CMOS e teve alteração do comportamento após troca de BIOS.

Classificação inicial: sintoma documentado; não reduzir a causa apenas à bateria CMOS.

### 5. Funciona uma vez e depois não liga; volta após reset RTC

Há relato específico de LA-B071P em que o equipamento funciona uma vez, depois não liga, e volta temporariamente após reset RTC.

No mesmo tópico foi sugerido verificar chipset.

Classificação inicial: caso documentado; causa ainda não confirmada.

### 6. Acende LED por aproximadamente um segundo e desliga; fan não parte

Há relato em fórum técnico de LA-B071P com determinado dump de BIOS em que a placa acende o LED de power por cerca de um segundo, apaga e o fan não inicia.

Classificação inicial: comportamento documentado em contexto de firmware; não afirmar que firmware é a causa original sem evidência adicional.

### 7. Power light indicator only

Há caso específico de LA-B071P descrito como apenas indicador de power, sem progressão normal de boot.

Classificação inicial: sintoma documentado.

### 8. BIOS/ME incorreto causando ausência de vídeo

Fonte técnica em polonês afirma que, em determinada configuração LA-B071P, ao trocar o BIOS normalmente é necessário preservar/transferir a região ME do dump original; caso contrário pode não haver imagem.

Classificação inicial: evidência técnica relevante, mas deve ser validada por revisão/CPU/firmware.

### 9. Pedidos recorrentes de Clear ME / rebuild de BIOS

Existem muitos registros de LA-B071P em fóruns pedindo `clear ME`, rebuild e reparo de firmware associados a:

- sem vídeo;
- vídeo demorado;
- não liga;
- comportamento intermitente.

Isso mostra recorrência de suspeita/intervenção em firmware, mas NÃO deve ser convertido em conclusão de que `ME` é causa comum sem análise dos casos e resultados.

### 10. BIOS password / senha de firmware

Há diversos casos de senha de BIOS. Algumas discussões indicam envolvimento do KBC/EC para armazenamento/remoção de senha.

Classificação inicial: problema documentado de firmware/configuração; pesquisar especificamente a arquitetura de armazenamento da senha nesta plataforma.

### 11. Service Tag / DMI persistente após regravação

Há relato de tentativa de alterar Service Tag com BIOS/ME regravado e permanência do identificador antigo.

Classificação inicial: problema de identidade/DMI documentado; pesquisar onde esses dados ficam efetivamente armazenados na plataforma.

### 12. BIOS incompatível entre Latitude 3450 e 3550

Há discussões sugerindo placa/família muito próxima ou compartilhada, mas tentativa de escrever BIOS de um modelo em outro pode falhar ou gerar comportamento incorreto.

Classificação inicial: risco documentado. Não misturar dumps sem confirmar modelo, revisão, CPU, EC e estrutura do firmware.

---

## Pesquisa obrigatória — cobertura completa

O OpenCode deve pesquisar a LA-B071P em múltiplos idiomas e fontes até produzir uma matriz por subsistema.

### Alimentação de entrada

Pesquisar:

- não liga;
- sem LED;
- curto na entrada;
- carregador cai;
- MOSFETs de entrada;
- charger IC;
- ACDET;
- ACOK;
- VIN;
- resistor shunt;
- identificação do carregador Dell;
- consumo zero;
- consumo alto imediato;
- liga e desliga.

### Rails always / standby

Pesquisar:

- ausência de 3V/5V standby;
- PWM de 3V/5V;
- enable;
- power good;
- curto em 3V/5V;
- alimentação do EC;
- RTC rail;
- sinais de reset.

### Sequência de power

Montar a sequência baseada no esquema real, sem inventar:

- VIN;
- always rails;
- EC/KBC;
- RTC;
- RSMRST#;
- SLP_Sx#;
- enables;
- SUS/MAIN rails;
- CPU rails;
- power-good;
- clock/reset;
- POST.

Registrar designadores reais e páginas do esquema quando disponíveis.

### BIOS / Intel ME / EC

Pesquisar:

- no display;
- late display;
- no power;
- ME corrupta;
- dump errado;
- versão A07/A08;
- REV 1.0/A00;
- rebuild;
- clean ME;
- DMI;
- Service Tag;
- senha BIOS;
- EC/KBC firmware;
- MEC5085;
- SPI ROM;
- comportamento após reset RTC.

### Vídeo / POST

Pesquisar:

- sem vídeo;
- cinco bipes;
- vídeo somente após segunda tentativa;
- display atrasado;
- backlight;
- eDP/LVDS conforme esquema;
- memória;
- CPU/PCH;
- LCD rail;
- clock/reset;
- POST travado.

### CPU / PCH / chipset

Pesquisar:

- chipset aquecendo;
- CPU fria;
- CPU aquecendo rapidamente;
- PCH/chipset;
- BGA;
- VRM;
- VCORE;
- sinais de enable/power-good;
- comportamento que exige reset RTC.

### Memória

Pesquisar:

- sem POST;
- um slot sem funcionar;
- DDR3 rail;
- VTT/VREF;
- slot danificado;
- RAM não reconhecida;
- beep code relacionado.

### Carregamento / bateria

Pesquisar:

- não carrega;
- não reconhece carregador;
- battery not charging;
- charger ID;
- SMBus;
- BATDRV;
- MOSFET de bateria;
- bateria detectada mas sem carga;
- só liga com/sem bateria.

### Portas e periféricos

Pesquisar:

- USB;
- HDMI;
- LAN;
- áudio;
- teclado;
- touchpad;
- Wi-Fi;
- webcam;
- SD;
- SATA;
- M.2, se aplicável à revisão/equipamento;
- fan.

### Térmica / estabilidade

Pesquisar:

- overheating;
- fan full speed;
- fan not spin;
- shutdown;
- restart;
- freeze;
- intermittent boot;
- only works after RTC reset;
- cold/hot fault.

### Danos físicos

Pesquisar relatos de:

- oxidação;
- líquido;
- trilha rompida;
- pad arrancado;
- conector DC;
- curto em capacitor;
- MOSFET em curto;
- dano por sobretensão.

---

## Consultas multilíngues obrigatórias

Usar pelo menos inglês, português, russo, polonês, vietnamita, hindi/inglês técnico de fóruns indianos, espanhol, turco e chinês quando houver resultados úteis.

Exemplos de busca:

- `"LA-B071P" no power`
- `"LA-B071P" no display`
- `"LA-B071P" 5 beeps`
- `"LA-B071P" charging`
- `"LA-B071P" short`
- `"LA-B071P" 3V 5V`
- `"LA-B071P" RTC`
- `"LA-B071P" clear ME`
- `"LA-B071P" late display`
- `"LA-B071P" power light only`
- `"LA-B071P" chipset`
- `"LA-B071P" MEC5085`
- `"LA-B071P" schematic`
- `"LA-B071P" boardview`
- equivalentes traduzidos para outros idiomas.

---

## Estrutura a criar

Criar somente após confirmar os vínculos de modelo/placa:

```text
MARCAS/DELL/
├── Latitude_3450/
│   └── PLACAS/LA-B071P/
└── Latitude_3550/
    └── PLACAS/LA-B071P/
```

Evitar duplicar conteúdo físico da placa. Se a mesma board ID/revisão realmente for compartilhada entre modelos, usar uma fonte técnica canônica da placa e referências cruzadas a partir dos modelos.

Sugestão canônica:

```text
PLACAS/COMPAL/LA-B071P/
├── 00_IDENTIFICACAO.md
├── 01_VARIANTES_REVISOES.md
├── 02_MAPA_SUBSISTEMAS.md
├── 03_SEQUENCIA_POWER.md
├── 04_COMPONENTES_CRITICOS.md
├── 05_FIRMWARE_BIOS_ME_EC.md
├── PROBLEMAS/
│   ├── NAO_LIGA.md
│   ├── SEM_VIDEO.md
│   ├── LATE_DISPLAY.md
│   ├── CINCO_BIPES.md
│   ├── LIGA_E_DESLIGA.md
│   ├── SO_LED_POWER.md
│   ├── SO_LIGA_APOS_RESET_RTC.md
│   ├── NAO_CARREGA.md
│   ├── BIOS_PASSWORD.md
│   └── ...
├── CASOS_REAIS.md
├── VIDEOS.md
├── FONTES.md
└── REVISOES.md
```

A arquitetura final deve ser debatida com o ChatGPT antes de migração estrutural grande.

---

## Debate obrigatório com o ChatGPT

Esta é a primeira missão usada para validar o modo de colaboração.

Depois do PRIMEIRO LEVANTAMENTO, o OpenCode NÃO deve consolidar tudo diretamente.

Deve enviar ao ChatGPT via MCP uma mensagem em modo `DEBATE` contendo:

```text
TECNICOPROMAX_MCP_MESSAGE
MODE: DEBATE
TASK: MISSAO_001_LA-B071P

RESUMO:
- identificação encontrada;
- revisões encontradas;
- modelos de notebook encontrados;
- componentes principais encontrados;
- sintomas documentados;
- causas alegadas;
- procedimentos encontrados;
- conflitos de fontes;
- quantidade de fontes;
- idiomas pesquisados.

QUESTOES PARA O CHATGPT:
1. Quais sintomas já têm evidência suficiente para virar arquivos específicos?
2. Quais causas ainda devem permanecer como hipótese?
3. Quais conflitos precisam de pesquisa adicional?
4. A estrutura proposta deve ser mantida ou alterada?
5. Quais lacunas técnicas precisam ser pesquisadas antes da consolidação?

OPENCODE_RECOMMENDATION:
<sua análise técnica>

RISK_IF_WRONG:
<impacto>
```

Aguardar até 300 segundos conforme `PROTOCOLO_CONVERSACAO_MCP.md`.

Se o ChatGPT pedir mais evidência, continuar pesquisando e retornar em nova rodada de debate.

---

## Trabalho contínuo

Enquanto aguarda resposta do ChatGPT, continuar apenas em tarefas independentes e seguras, por exemplo:

- coletar novas fontes;
- catalogar URLs;
- traduzir material já coletado;
- localizar datasheets;
- comparar revisões;
- montar inventário de componentes.

NÃO consolidar uma decisão que esteja bloqueada pelo debate.

NÃO ficar ocioso se houver pesquisa independente útil disponível.

---

## Critério de conclusão

A missão NÃO termina quando forem encontrados 5 ou 10 defeitos.

Só pode ser marcada como consolidada após existir:

- identificação da placa;
- variantes/revisões documentadas;
- matriz de todos os subsistemas;
- lista de sintomas específicos encontrados;
- lista de falhas possíveis ainda sem caso específico;
- árvore de diagnóstico por sintoma relevante;
- componentes críticos e seus datasheets;
- sequência de power baseada em esquema;
- seção BIOS/ME/EC;
- fontes completas;
- vídeos relevantes;
- conflitos e incertezas;
- pelo menos uma rodada de debate ChatGPT ↔ OpenCode;
- auditoria final do ChatGPT.

Não existe garantia lógica de enumerar literalmente todo defeito físico concebível; a meta é cobertura técnica abrangente e rastreável.

---

## Estado inicial da missão

- Status: INICIADA
- Board ID: LA-B071P
- Prioridade: máxima
- Modo MCP requerido: DEBATE
- Executor: OpenCode/Ben Code
- Comandante técnico: ChatGPT
- Usuário: autoridade final
