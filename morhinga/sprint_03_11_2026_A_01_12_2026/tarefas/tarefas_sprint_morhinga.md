# Morhinga — Sprint de evolução do monitoramento ambiental

**Período:** 03/11/2026 a 01/12/2026. **Equipe:** Jorge (backend), Ryan (frontend e integração), Thiago (frontend e UI/UX). **Meta:** permitir que o usuário interprete dados reais, compare períodos e identifique leituras antigas ou valores fora de limites operacionais, com uma experiência coerente em celular e desktop.

## 1\. Review e diagnóstico

O documento de outubro descreve um plano para chatbot, PDF e CSV. Ele não é comprovante de conclusão. A revisão abaixo usa os arquivos locais de publicação e o registro de verificação de 03/11, sem executar novamente os testes, consultar o Firebase ou consumir cota Gemini.

| Frente | Evidência encontrada | Implicação para novembro |
| :---- | :---- | :---- |
| Arquitetura | React/Vite, Recharts e CSS Modules; Django com controllers, services, repositories e domínio | Evoluir os serviços e hooks existentes, evitando reconstrução |
| Histórico e arquivos | Controllers de CSV e PDF com filtros e verificação de acesso | Consolidar e corrigir consistência, em vez de criar os mesmos botões novamente |
| Relatório visual | demoReportService.js contém KPIs e séries fixos; README avisa que getReportData é demonstrativo | Remover demonstração do uso normal e ligar a análise a dados reais |
| Unidades | exports.py usa turbidez\_v como alternativa a turbidez\_ntu no CSV e no PDF | Corrigir antes de comparar ou divulgar dados. Volts não equivalem a NTU |
| Gráficos | Dashboard.jsx recorta as últimas 20 leituras para apresentação | Criar comparação de período com contrato próprio, sem tratar 20 pontos como histórico completo |
| Histórico no backend | TelemetryService lê dados do equipamento e filtra em memória | Medir volume e limitar intervalo nesta sprint. Consulta paginada/indexada completa fica para depois, salvo bloqueio real |
| Chat | Serviços e hook separados; quota persistente de 5 tentativas por UID/dia | Preservar contexto e erros. Não transformar o mês em expansão de IA |
| Validação prévia | VERIFICACAO-LOCAL.md registra 769 testes backend e 4 frontend aprovados, build/lint e healthcheck | Resultados históricos locais, não certificação da versão publicada |
| Pendências reais | Gemini respondeu 503 nos testes reais registrados. Login real e revisão visual por navegador não foram concluídos | Validar integração na primeira semana e não prometer chat real já homologado |

**Estado de publicação:** há cópias em analise/ e publicacao/. A revisão não confirma qual commit está no ambiente público. J01/R01 devem fixar a versão de referência e conferir diferenças antes de iniciar features. Não houve nova publicação nesta atividade de planejamento.

**Adequação à equipe:** a distribuição anterior indica familiaridade de Jorge com Django/Firebase e APIs, de Ryan com serviços/componentes e estados de integração, e de Thiago com CSS Modules e responsividade. Isso orienta as tarefas, sem inferir senioridade ou produtividade individual. Cada aluno terá uma evolução técnica próxima do que já fez, com revisão conjunta nas segundas.

## 2\. Entrega que fará diferença no site

1. **Análise real por equipamento e período:** mínimo, máximo, média, quantidade válida e unidades consistentes. O relatório visual deixa de exibir indicadores demonstrativos.  
2. **Comparação de dois períodos:** mesmo equipamento, mesma variável e intervalos de igual duração. Duas séries e diferença absoluta da média ajudam a perceber mudanças.  
3. **Confiança na leitura:** horário da última medição, idade, dados inválidos e estado atualizado/desatualizado/sem dados/indeterminado. Erro de rede aparece como erro de consulta.  
4. **Avisos operacionais no dashboard:** até duas variáveis homologadas por tipo de equipamento, com regra configurada no backend. O usuário vê o valor e o limite que motivaram o aviso.  
5. **Uso móvel e exportação coerente:** filtros aplicados compartilhados, teclado, unidades legíveis e CSV/PDF alinhados ao contexto analisado.

Exemplo da demonstração: selecionar uma estação, analisar sete dias, comparar com os sete anteriores, conferir uma leitura antiga e baixar o PDF/CSV da seleção. Repetir com uma boia e mostrar suas próprias variáveis. Os cenários de aviso e erro podem usar fixtures sintéticas identificadas, sem gravar dados fictícios no Firebase de produção.

**Limites de escopo:** sem previsão do tempo, previsão por IA, novo hardware, novos sensores, envio de email/WhatsApp, processamento contínuo de alertas, editor de regras no frontend ou comparação entre vários equipamentos. Avisos são calculados na consulta, sem histórico persistente de incidentes. Chat mantém contexto e quota atuais. Qualquer extra só entra após aceite do escopo principal.

## 3\. Capacidade, rotina e calendário

São quatro ciclos de compromisso de **20h por estudante**, totalizando **80h por pessoa e 240h da equipe**. A entrega em 01/12 usa o orçamento do quarto ciclo e não cria uma quinta semana de 20h. Horas são esforço, não tempo corrido. Os dias de trabalho podem ter carga diferente, desde que o limite do ciclo seja respeitado.

Premissas: dailys de 15 minutos nos dias de atividade, planejamento/demonstração de 1h nas terças de novembro, revisão e correção de código de 2h nas segundas, e aceite final de 30 minutos em 01/12. Não há trabalho obrigatório em fins de semana nem em 20/11, reservado como indisponível neste planejamento. Confirmar calendário acadêmico na abertura; outras indisponibilidades reduzem escopo, sem compensação automática fora das 20h.

| Ciclo | Período | Desenvolvimento por aluno | Cerimônias e review por aluno | Reserva por aluno | Total por aluno |
| :---- | :---- | ----: | ----: | ----: | ----: |
| 1 | 03–09/11 | 14h | 4,25h | 1,75h | 20h |
| 2 | 10–16/11 | 14h | 4,25h | 1,75h | 20h |
| 3 | 17–23/11 | 13h | 4h | 3h | 20h |
| 4 | 24/11–01/12 | 13h | 5h | 2h | 20h |
| **Total** |  | **54h** | **17,5h** | **8,5h** | **80h** |

Os ciclos têm respectivamente 5, 5, 4 e 6 dias de atividade. As dailys somam 5h por pessoa. As terças de novembro somam 4h de planejamento, as quatro segundas somam 8h de revisão/correção e o aceite soma 0,5h. A reserva cobre imprevistos e aprendizado, não novas features. A equipe dispõe de 162h em tarefas, 52,5h em rotina/review e 25,5h de reserva.

**Segundas (09, 16, 23 e 30/11):** revisar PRs, corrigir regressões, integrar e atualizar evidências. Sem iniciar features. As tarefas de fechamento em 30/11 usam apenas correção/documentação e não duplicam as 2h de revisão reservadas.

**Terças:** 03/11 abertura e baseline; 10/11 demonstração da base e planejamento da análise; 17/11 demonstração de dados reais/comparação e planejamento de qualidade; 24/11 demonstração de avisos e fechamento do escopo; 01/12 demonstração e aceite final. Daily informa entrega anterior, próximo passo e bloqueio. Bloqueio acima de um dia exige replanejamento, fixtures de contrato ou pareamento usando a reserva.

| Marco | Evidência necessária |
| :---- | :---- |
| 09/11 | Baseline registrada, contratos fechados, resumo backend e componentes com fixtures |
| 16/11 | Relatório visual real e comparação integrados, PDF/CSV coerentes |
| 23/11 | Qualidade e avisos integrados, experiência móvel revisada |
| 27/11 | Release candidata e roteiro completo, congelamento de funcionalidades |
| 30/11 | Revisão final, correções e decisão de liberação |
| 01/12 | Demonstração, aceite, documentação e lista explícita de pendências |

## 4\. Cadastro no Base44

Cada linha abaixo corresponde a um registro. Campos sugeridos: ID, título, responsável, início, prazo, estimativa\_horas, prioridade, ciclo, dependências, descrição, critérios\_de\_aceite e status. Status inicial: A fazer. IDs referenciados nas dependências devem virar relações entre registros, se sua base suportar. Relação não significa trabalho adicional do colaborador citado.

P0 representa entrega essencial. P1 representa avisos por limites, planejados mas removíveis antes de comprometer dados reais, comparação, qualidade e estabilização. Datas usam o ano de 2026 e o fuso America/Sao\_Paulo. Dependências de contrato permitem trabalho com fixtures. Integração real exige a API e os componentes concluídos; atividades no mesmo dia devem combinar uma entrega intermediária na daily.

| ID | Título | Responsável | Início | Prazo | Horas | Prioridade | Ciclo | Dependências |
| :---- | :---- | :---- | :---- | :---- | ----: | :---- | ----: | :---- |
| J01 | Auditar entrega e catálogo de variáveis | Jorge | 03/11/2026 | 04/11/2026 | 4 | P0 | 1 | — |
| J02 | Definir contrato de análise e qualidade | Jorge | 04/11/2026 | 05/11/2026 | 4 | P0 | 1 | J01 |
| J03 | Implementar resumo estatístico real | Jorge | 05/11/2026 | 06/11/2026 | 6 | P0 | 1 | J02 |
| J04 | Entregar comparação entre dois períodos | Jorge | 10/11/2026 | 12/11/2026 | 8 | P0 | 2 | J03 |
| J05 | Unificar PDF, CSV e relatório visual | Jorge | 12/11/2026 | 13/11/2026 | 6 | P0 | 2 | J03 |
| J06 | Calcular qualidade e idade da última leitura | Jorge | 17/11/2026 | 18/11/2026 | 7 | P0 | 3 | J02, J05 |
| J07 | Entregar avisos por limites operacionais | Jorge | 18/11/2026 | 19/11/2026 | 6 | P1 | 3 | J06 |
| J08 | Validar contratos e isolamento de acesso | Jorge | 24/11/2026 | 25/11/2026 | 5 | P0 | 4 | J04, J05, J06, J07, R07 |
| J09 | Preparar release e roteiro de reversão | Jorge | 26/11/2026 | 27/11/2026 | 5 | P0 | 4 | J08 |
| J10 | Fechar documentação e demonstração backend | Jorge | 30/11/2026 | 01/12/2026 | 3 | P0 | 4 | J09, R10, T10 |
| R01 | Mapear fluxos e pendências de integração | Ryan | 03/11/2026 | 04/11/2026 | 4 | P0 | 1 | — |
| R02 | Criar serviço e hook de análise | Ryan | 05/11/2026 | 05/11/2026 | 4 | P0 | 1 | J02, R01 |
| R03 | Centralizar equipamento e período dos fluxos | Ryan | 06/11/2026 | 06/11/2026 | 6 | P0 | 1 | R02, T02 |
| R04 | Conectar relatório visual aos dados reais | Ryan | 10/11/2026 | 12/11/2026 | 8 | P0 | 2 | J03, R03, T03 |
| R05 | Integrar comparação de períodos | Ryan | 12/11/2026 | 13/11/2026 | 6 | P0 | 2 | J04, R03, T04 |
| R06 | Integrar estado de atualização e qualidade | Ryan | 17/11/2026 | 18/11/2026 | 7 | P0 | 3 | J06, R04, T05 |
| R07 | Integrar lista de avisos operacionais | Ryan | 18/11/2026 | 19/11/2026 | 6 | P1 | 3 | J07, R06, T06 |
| R08 | Executar integração ponta a ponta | Ryan | 24/11/2026 | 25/11/2026 | 6 | P0 | 4 | R04, R05, R06, R07, J05 |
| R09 | Corrigir falhas de integração prioritárias | Ryan | 26/11/2026 | 27/11/2026 | 4 | P0 | 4 | R08, J08, T08 |
| R10 | Concluir guia de uso e ensaio integrado | Ryan | 30/11/2026 | 01/12/2026 | 3 | P0 | 4 | R09 |
| T01 | Auditar experiência e identidade visual | Thiago | 03/11/2026 | 04/11/2026 | 4 | P0 | 1 | — |
| T02 | Definir layouts e estados da evolução | Thiago | 04/11/2026 | 05/11/2026 | 4 | P0 | 1 | T01, J02 |
| T03 | Criar componentes visuais de resumo e filtros | Thiago | 05/11/2026 | 06/11/2026 | 6 | P0 | 1 | T02 |
| T04 | Construir visualização comparativa responsiva | Thiago | 10/11/2026 | 13/11/2026 | 8 | P0 | 2 | T03, J02 |
| T05 | Construir estados de qualidade e atualização | Thiago | 12/11/2026 | 13/11/2026 | 6 | P0 | 2 | T02 |
| T06 | Construir interface de avisos por limites | Thiago | 17/11/2026 | 18/11/2026 | 7 | P1 | 3 | T05, J02 |
| T07 | Revisar acessibilidade e navegação móvel | Thiago | 18/11/2026 | 19/11/2026 | 6 | P0 | 3 | T04, T05, T06, R04 |
| T08 | Executar revisão visual dos fluxos completos | Thiago | 24/11/2026 | 25/11/2026 | 6 | P0 | 4 | T07, R07, R08 |
| T09 | Corrigir inconsistências visuais de entrega | Thiago | 26/11/2026 | 27/11/2026 | 4 | P0 | 4 | T08 |
| T10 | Preparar material visual e demonstração final | Thiago | 30/11/2026 | 01/12/2026 | 3 | P0 | 4 | T09, R09 |

## 5\. Descrição e aceite de cada tarefa

### J01 — Auditar entrega e catálogo de variáveis

**Responsável:** Jorge. **Janela:** 03/11 a 04/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** —.

**Trabalho:** Executar checklist de login, equipamentos, histórico, CSV, PDF e chat. Registrar o que funciona, o que está pendente e as variáveis reais por tipo de equipamento.

**Aceite:** Matriz com evidências, unidades e pendências. Nenhuma chave exposta. Teste Gemini real somente com autorização vigente.

### J02 — Definir contrato de análise e qualidade

**Responsável:** Jorge. **Janela:** 04/11 a 05/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** J01.

**Trabalho:** Definir JSON de resumo, comparação e qualidade, reaproveitando rotas existentes. Publicar exemplos sintéticos para Ryan e Thiago.

**Aceite:** Contrato versionado com autenticação, filtros, unidades, erros, nulos e limites. Separar turbidez em volts e NTU.

### J03 — Implementar resumo estatístico real

**Responsável:** Jorge. **Janela:** 05/11 a 06/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** J02.

**Trabalho:** Criar serviço reutilizável de mínimo, máximo, média e contagem por variável e período. Expor resposta JSON autenticada para o relatório visual.

**Aceite:** Testes com estação e boia, ausência de dados, zero válido e acesso negado. Média exclui nulos e inválidos com contagem explícita.

### J04 — Entregar comparação entre dois períodos

**Responsável:** Jorge. **Janela:** 10/11 a 12/11/2026. **Estimativa:** 8h. **Prioridade:** P0. **Dependências:** J03.

**Trabalho:** Comparar o mesmo equipamento e variável em dois intervalos de igual duração, calculando média e diferença absoluta. Limitar consultas ao período suportado.

**Aceite:** Valores conferidos com fixture manual. Sem dados produz indisponível. Sem percentuais para pH ou base zero. Unidades iguais em ambos os períodos.

### J05 — Unificar PDF, CSV e relatório visual

**Responsável:** Jorge. **Janela:** 12/11 a 13/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** J03.

**Trabalho:** Reutilizar regras de filtro e validação. Corrigir a mistura de turbidez\_v com turbidez\_ntu observada nos exports. Tratar tabela PDF em várias páginas.

**Aceite:** Mesmo equipamento, período, unidade e contagens válidas na tela e arquivos. CSV preserva registros e nulos; PDF não corta tabelas. API.md inclui rotas de exportação reais.

### J06 — Calcular qualidade e idade da última leitura

**Responsável:** Jorge. **Janela:** 17/11 a 18/11/2026. **Estimativa:** 7h. **Prioridade:** P0. **Dependências:** J02, J05.

**Trabalho:** Retornar horário da última leitura válida, idade, quantidade inválida e status atualizado/desatualizado/sem dados/indeterminado. Usar tolerância configurada por equipamento.

**Aceite:** Limites temporais e fuso testados. Cadência desconhecida resulta em indeterminado. Sem confundir leitura antiga com falha de rede ou equipamento offline.

### J07 — Entregar avisos por limites operacionais

**Responsável:** Jorge. **Janela:** 18/11 a 19/11/2026. **Estimativa:** 6h. **Prioridade:** P1. **Dependências:** J06.

**Trabalho:** Aplicar configuração simples no backend para até duas variáveis homologadas por tipo de equipamento. Avaliar a última leitura válida com limites inferior e superior.

**Aceite:** Aviso informa variável, unidade, horário, valor e regra. Leitura antiga não recebe estado normal. Sem regra resulta em não configurado. Sem parecer de conformidade ambiental.

### J08 — Validar contratos e isolamento de acesso

**Responsável:** Jorge. **Janela:** 24/11 a 25/11/2026. **Estimativa:** 5h. **Prioridade:** P0. **Dependências:** J04, J05, J06, J07, R07.

**Trabalho:** Executar testes de regressão de API e casos integrados com a equipe. Corrigir primeiro qualquer vazamento de equipamento ou divergência de unidade.

**Aceite:** Evidências de 401/403, filtros e casos sem dados. Chat não mistura equipamentos. Falha e cota Gemini possuem resposta controlada.

### J09 — Preparar release e roteiro de reversão

**Responsável:** Jorge. **Janela:** 26/11 a 27/11/2026. **Estimativa:** 5h. **Prioridade:** P0. **Dependências:** J08.

**Trabalho:** Conferir versões do ambiente de destino, variáveis, migrações e healthcheck. Documentar publicação e reversão, com exemplo de configuração de regras.

**Aceite:** Checklist executável, versões registradas e release candidata identificada. Publicação depende do acesso de implantação já autorizado à equipe.

### J10 — Fechar documentação e demonstração backend

**Responsável:** Jorge. **Janela:** 30/11 a 01/12/2026. **Estimativa:** 3h. **Prioridade:** P0. **Dependências:** J09, R10, T10.

**Trabalho:** Na segunda, corrigir falhas de revisão e fechar contratos. Na terça, apresentar consultas e artefatos do cenário de aceite.

**Aceite:** README e API atualizados, evidências vinculadas e nenhuma falha bloqueadora aberta. Demonstração final usa dados reais disponíveis.

### R01 — Mapear fluxos e pendências de integração

**Responsável:** Ryan. **Janela:** 03/11 a 04/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** —.

**Trabalho:** Conferir rotas, hooks, serviços e fluxo de troca de equipamento. Identificar onde getReportData demonstrativo chega à interface.

**Aceite:** Checklist reproduzível de estados e chamadas, incluindo login, exportação e erro de cota.

### R02 — Criar serviço e hook de análise

**Responsável:** Ryan. **Janela:** 05/11 a 05/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** J02, R01.

**Trabalho:** Consumir contrato de resumo com transporte comum e token. Iniciar com fixtures do contrato para não depender da entrega final da API.

**Aceite:** Cancelamento ou descarte de respostas antigas ao trocar equipamento. Loading, erro e sem dados separados.

### R03 — Centralizar equipamento e período dos fluxos

**Responsável:** Ryan. **Janela:** 06/11 a 06/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** R02, T02.

**Trabalho:** Compartilhar seleção entre dashboard, histórico, relatório e downloads, preservando assinaturas existentes onde possível.

**Aceite:** Troca de equipamento limpa dados anteriores. Datas enviadas nos formatos acordados. Exportação usa a pesquisa aplicada, mesmo se formulário mudar depois.

### R04 — Conectar relatório visual aos dados reais

**Responsável:** Ryan. **Janela:** 10/11 a 12/11/2026. **Estimativa:** 8h. **Prioridade:** P0. **Dependências:** J03, R03, T03.

**Trabalho:** Substituir o fluxo demonstrativo de produção por resumo real e gráficos das variáveis disponíveis. Retirar KPIs fixos sem suporte, como 98% ativos.

**Aceite:** Nenhum valor demonstrativo no uso normal. Estação e boia mostram variáveis próprias. Arquivos respeitam a seleção aplicada.

### R05 — Integrar comparação de períodos

**Responsável:** Ryan. **Janela:** 12/11 a 13/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** J04, R03, T04.

**Trabalho:** Adicionar escolha de período atual e anterior de igual duração. Mostrar duas séries compatíveis e diferença absoluta da média.

**Aceite:** Casos sem dados ou unidade incompatível ficam claros. Uma variável por gráfico, com unidade e legenda. Resposta atrasada não altera seleção nova.

### R06 — Integrar estado de atualização e qualidade

**Responsável:** Ryan. **Janela:** 17/11 a 18/11/2026. **Estimativa:** 7h. **Prioridade:** P0. **Dependências:** J06, R04, T05.

**Trabalho:** Mostrar última leitura, idade, qualidade e atualização manual. Buscar qualidade ao abrir e ao atualizar, sem polling nesta entrega.

**Aceite:** Distinguir erro da API, ausência de dados e leitura antiga. Requisição repetida não duplica ação nem apaga a última leitura sem explicação.

### R07 — Integrar lista de avisos operacionais

**Responsável:** Ryan. **Janela:** 18/11 a 19/11/2026. **Estimativa:** 6h. **Prioridade:** P1. **Dependências:** J07, R06, T06.

**Trabalho:** Apresentar avisos da última leitura do equipamento selecionado. Incluir variável, valor, horário, regra e orientação descritiva.

**Aceite:** Sem avisos, regra ausente e dados antigos têm mensagens distintas. Sem avisos de outro equipamento e sem promessa de notificação externa.

### R08 — Executar integração ponta a ponta

**Responsável:** Ryan. **Janela:** 24/11 a 25/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** R04, R05, R06, R07, J05.

**Trabalho:** Validar login, troca rápida de equipamento, comparação, histórico e download. Incluir falha API, 429 do chat e acesso negado.

**Aceite:** Roteiro registrado com resultado esperado/obtido. Build, lint e testes existentes passam no ambiente definido.

### R09 — Corrigir falhas de integração prioritárias

**Responsável:** Ryan. **Janela:** 26/11 a 27/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** R08, J08, T08.

**Trabalho:** Resolver falhas bloqueadoras e de seleção/filtros. Medir tamanho do bundle antes e depois e aplicar importação sob demanda apenas se o fluxo afetado justificar.

**Aceite:** Fluxos críticos sem erros de console, sem regressão de download e sem troca indevida de contexto.

### R10 — Concluir guia de uso e ensaio integrado

**Responsável:** Ryan. **Janela:** 30/11 a 01/12/2026. **Estimativa:** 3h. **Prioridade:** P0. **Dependências:** R09.

**Trabalho:** Documentar filtros, comparação e exportações com capturas da versão candidata. Na segunda, incorporar correções de review.

**Aceite:** Guia curto disponível e roteiro final executado com Jorge e Thiago.

### T01 — Auditar experiência e identidade visual

**Responsável:** Thiago. **Janela:** 03/11 a 04/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** —.

**Trabalho:** Revisar dashboard, histórico, relatório e chat em desktop e celular. Levantar problemas de contraste, espaço, navegação e estados vazios.

**Aceite:** Lista priorizada com capturas. Preservar azul profundo, ciano e identidade Morhinga sem redesenhar toda a aplicação.

### T02 — Definir layouts e estados da evolução

**Responsável:** Thiago. **Janela:** 04/11 a 05/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** T01, J02.

**Trabalho:** Desenhar hierarquia do período, resumo, comparação e qualidade. Alinhar com Ryan os componentes e textos.

**Aceite:** Layouts desktop e 360 px, com loading, erro, sem dados e desatualizado. Nenhuma informação depende somente de cor.

### T03 — Criar componentes visuais de resumo e filtros

**Responsável:** Thiago. **Janela:** 05/11 a 06/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** T02.

**Trabalho:** Implementar apresentação reutilizável de estatísticas, unidades e período aplicado, com fixtures sintéticas identificadas no desenvolvimento.

**Aceite:** Props acordadas com Ryan, foco visível e labels. Sem regras estatísticas dentro dos componentes.

### T04 — Construir visualização comparativa responsiva

**Responsável:** Thiago. **Janela:** 10/11 a 13/11/2026. **Estimativa:** 8h. **Prioridade:** P0. **Dependências:** T03, J02.

**Trabalho:** Estilizar controles, legenda e apresentação de duas séries para uma variável. Evidenciar diferença absoluta e períodos comparados.

**Aceite:** Gráfico legível em 360, 768 e 1366 px, com alternativa textual de médias e diferença. Sem misturar eixos de temperatura e umidade.

### T05 — Construir estados de qualidade e atualização

**Responsável:** Thiago. **Janela:** 12/11 a 13/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** T02.

**Trabalho:** Criar apresentação de horário, idade e rótulo de qualidade com textos e indicadores acessíveis. Usar fixtures de cada estado.

**Aceite:** Estados atualizado, antigo, sem dados, indeterminado e erro têm texto distinto. Não usar offline sem evidência técnica.

### T06 — Construir interface de avisos por limites

**Responsável:** Thiago. **Janela:** 17/11 a 18/11/2026. **Estimativa:** 7h. **Prioridade:** P1. **Dependências:** T05, J02.

**Trabalho:** Criar lista de avisos operacionais com variável, unidade, data e limite, além de estados sem regra e sem leitura recente.

**Aceite:** Sem linguagem de risco à saúde ou conformidade legal. Cores acompanhadas de texto e ícone existente da aplicação.

### T07 — Revisar acessibilidade e navegação móvel

**Responsável:** Thiago. **Janela:** 18/11 a 19/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** T04, T05, T06, R04.

**Trabalho:** Corrigir navegação por teclado, foco de modais, labels, contraste e largura dos gráficos. Revisar chat e downloads existentes.

**Aceite:** Percurso completo por teclado, fechamento de modal e retorno do foco. Sem rolagem horizontal da página em 360 px.

### T08 — Executar revisão visual dos fluxos completos

**Responsável:** Thiago. **Janela:** 24/11 a 25/11/2026. **Estimativa:** 6h. **Prioridade:** P0. **Dependências:** T07, R07, R08.

**Trabalho:** Inspecionar versão integrada em 360, 768 e 1366 px com estação e boia. Registrar capturas e defeitos por severidade.

**Aceite:** Tela com dados, sem dados, carregando, erro e leitura antiga revisada. Critérios visuais reproduzíveis.

### T09 — Corrigir inconsistências visuais de entrega

**Responsável:** Thiago. **Janela:** 26/11 a 27/11/2026. **Estimativa:** 4h. **Prioridade:** P0. **Dependências:** T08.

**Trabalho:** Resolver cortes, sobreposições, rótulos sem unidade e estados ambíguos. Conferir leitura dos gráficos no tema escuro.

**Aceite:** Lista visual sem bloqueadores e componentes consistentes nas telas alteradas.

### T10 — Preparar material visual e demonstração final

**Responsável:** Thiago. **Janela:** 30/11 a 01/12/2026. **Estimativa:** 3h. **Prioridade:** P0. **Dependências:** T09, R09.

**Trabalho:** Na segunda, ajustar itens de review e selecionar capturas antes/depois. Preparar percurso de demonstração em celular e desktop.

**Aceite:** Evidências da mudança visual e roteiro de até 10 minutos alinhado com a equipe.

## 6\. Regras técnicas que evitam retrabalho

- O backend concentra validações, estatísticas, comparação e avisos. React recebe respostas e apresenta estados. Reutilizar httpTransport/httpClient, serviços e hooks existentes.  
- Reutilizar as rotas de histórico/CSV/PDF/chat atuais. Uma rota JSON de análise e campos de qualidade são propostas a fechar em J02, sem quebrar contratos existentes. Documentação atual de API não enumera todas as rotas de exportação.  
- Todo endpoint privado valida token e autorização do equipamento. O frontend nunca lê credenciais privadas Firebase ou Gemini. Fluxos públicos mantêm somente os dados já autorizados.  
- Um contrato define equipamento, início, fim, variável e fuso. O fim é inclusivo na experiência do usuário; a implementação deve tratar frações de segundo sem excluir o último instante do dia.  
- Zero é valor válido. Nulo, booleano, NaN e valor inválido não viram zero. Estatísticas informam quantidade válida por variável e descartes. Datas inválidas não entram nas séries. Não interpolar lacunas como medições reais.  
- Volts e NTU são grandezas distintas. Só converter com calibração documentada e validada; nesta sprint, apresentar separadamente. Validar também unidades e significado de gás, ar, vento e chuva antes de rotular. Chuva acumulada não deve ser somada como se cada ponto fosse precipitação independente.  
- A comparação usa diferença absoluta de médias e apresenta cobertura/contagem. Não chamar diferença de tendência preditiva. Sem leituras ou com amostragem incomparável, informar a limitação.  
- A tolerância de atraso depende da cadência homologada do equipamento. Sem cadência conhecida, usar indeterminado. Não inventar disponibilidade percentual de equipamentos sem uma base que sustente o cálculo.  
- Limites operacionais exigem variável e unidade confirmadas. A configuração inicial fica no backend; não usar limites legais ou de saúde sem validação específica. A interface mostra a origem da regra, sem declarar água potável ou ambiente seguro.  
- Começar com máximo de 30 dias por período e limite de pontos a definir em J02 após medir volume. Documentar agregação e total original. Séries agregadas não substituem o CSV bruto. O orçamento comporta uma otimização pontual se a leitura integral bloquear os fluxos, não uma migração ampla de armazenamento.

## 7\. Dependências, riscos e ajuste de escopo

O caminho principal passa por J02, J03, R04 e integração de J04/R05. Qualidade depende de J06 e integração R06. Avisos dependem de J07 e R07, com componentes de Thiago preparados antes da API. Ryan e Thiago usam fixtures de contrato durante o desenvolvimento paralelo; mocks não contam como aceite integrado.

| Risco/gatilho | Resposta dentro da capacidade |
| :---- | :---- |
| Baseline diferente do que está publicado | J01/R01 fixam a referência; usar reserva do ciclo 1 para pendências essenciais |
| Atraso superior a um dia no contrato | Reduzir opções de UI e parear dentro da reserva; manter exemplos JSON estáveis |
| Variáveis/unidades ou cadência desconhecidas | Mostrar indisponível/indeterminado e desabilitar aviso específico. Não inventar conversões ou limites |
| Consulta de 30 dias muito lenta ou muito grande | Medir em conjunto de referência, reduzir janela ou aplicar agregação documentada. Sem truncamento silencioso |
| Gemini indisponível | Preservar erro controlado e informar limitação. Não bloquear o restante da entrega nem consumir novas chamadas de teste sem autorização |
| Semana reduzida ou mais pendências que a reserva | Cortar P1 J07/R07/T06 como conjunto (19h da equipe); realocar essas horas para P0 e registrar a decisão |
| Problemas encontrados após 27/11 | Corrigir bloqueadores, sem abrir novas features. Se preciso, retirar P1 antes de reduzir validação |
| Sem equipamentos reais acessíveis | Executar fixtures identificadas e registrar aceite real como pendente. Não apresentar como integração comprovada |

**Meta de desempenho proposta:** medir um cenário de referência com equipamento, volume, ambiente e conexão registrados, buscando resposta de análise em até 3 segundos para a janela homologada. Esse é um alvo de aceite a calibrar em J02, não desempenho atual comprovado.

## 8\. Aceite da entrega de 01/12

A demonstração deve durar até 10 minutos e mostrar:

1. Login, escolha de uma estação e período, com dados reais disponíveis.  
2. Resumo real com unidade, mínimo/máximo/média e contagem válida.  
3. Comparação com intervalo anterior equivalente e diferença absoluta conferível.  
4. CSV e PDF com equipamento e período corretos, sem misturar volts com NTU.  
5. Estado de atualização e ausência de dados. Cenários simulados devem estar identificados.  
6. Aviso operacional com regra e valor, caso P1 permaneça no escopo.  
7. Uma boia com campos próprios, sem herdar valores da estação.  
8. Uso em 360 px e navegação por teclado, com estados de erro compreensíveis.  
9. Chat com isolamento de contexto e tratamento de quota/falha. Resposta real Gemini só conta como validada se houver execução autorizada bem-sucedida.

**Concluído significa:** PR revisado, código integrado na versão candidata, testes pertinentes e verificações existentes executados, fluxos de permissão e filtros conferidos, interface revisada, documentação atualizada e evidências associadas. Ausência de vazamento de dados, unidade errada ou quebra de fluxo principal é obrigatória. Publicação e healthcheck devem ser registrados se houver acesso ao ambiente de destino; caso contrário, entregar release pronta e marcar implantação como pendência, sem alegar site atualizado.

&nbsp;